
# LLisp: Lisp in Lisp



先週、私はあることを考えた。"私が書ける最も簡単なLispインタプリタで、マクロをサポートしているものは何か？"

週末にClojureをハックし、いくつかの混乱を経て、REPLが誕生しました。 このエッセイでは、同じ旅を経て、Clojureでマクロをサポートする独自のLispインタプリタを作ってみましょう! LLisp: a lisp in a lispとでも呼びましょうか。

最後に、あなたは自分のマクロを自分のプログラミング言語で書くことができます。さあ、始めましょう。

# スケッチ

さて、これから行うべきことの基本的な考え方をスケッチしてみましょう。


![image](/api/image/aHR0cHM6Ly91c2VyLWltYWdlcy5naXRodWJ1c2VyY29udGVudC5jb20vOTg0NTc0LzE1MDY1NTc1NC1lMTg0NmZlNC0yM2I3LTQ3ZTctYjJmNy1lMjJiMDJlZmU2NmQucG5n){style="max-width:100%"}


プログラマーが何かコードを書く。ここまでは単なるテキストで、私たちができることはあまりない。私たちはプログラマーの書いたテキストを読み、それをデータ構造に変換する。データ構造を評価し、プログラマが意図したものを返すことができるのだ。

もし、`"(+ 1 1)"` を `2` に変換することができれば、私たちはプログラミング言語のルーツを手に入れることができるのです。

# Reader

2.読み」を処理してみよう。プログラマが `"(+ 1 1)"` のようなテキストを書いたら、それをデータ構造に変換してあげたい。みたいな感じ。

```
(read-string "(+ 1 1)")
; =>
(+ 1 1)
```

自分たちで `read-string` を書くことは *可能* です。最も単純なケースでは、それは非常に簡単です^\[1\]^。しかし、我々は結局Clojureにいる。そして、ClojureはすでにLispコードの読み方を理解している。ズルしてClojureの `edn` を使おう。

```
(ns simple-lisp.core
  (:refer-clojure :exclude [eval read read-string])
  (:require [clojure.edn :refer [read read-string]]))
```

そして、ほら、`read-string` は私たちが望むことをします。

```
(read-string "(+ 1 1)")
; =>
(+ 1 1)
```

# Evaluation

さて、次は「3.評価」です。我々のゴールはデータ構造を受け取り、それを評価し、プログラマーが意図したものを返すことだ。まずは、こんな感じの簡単な関数から始めてみましょう。

```
(defn eval [form] 
  ;; TODO 
  )
```

## リテラルを評価する

簡単なことから始めましょう。例えば、プログラマが `12` と書いたら、 `12` という数字を返すだけでよいのです。 これは文字列(`"foo"`)、文字(`\b`)、ブーリアン(`true`)についても同じことが言えます。 これらはリテラル*.*です。

ここで、リテラルを評価する方法を説明します。まず、それらを検出しましょう。

```
(def literal?
  (some-fn string? number? boolean? char? nil?))
```

もしあなたが `some-fn` を見たことがなければ、これは clojure の数ある `便利で強力な` ユーティリティのうちの一つです。これで `eval` でリテラルを扱えるようになりました。

```
(defn eval [form]
  (cond
    (literal? form) form))
```

そして、私たちのプログラミング言語が何かをし始めるのです。

```
(eval (read-string "12"))
; => 12
```

## 引用の紹介

それでは、`quote`について説明します。 quoteはLispでは非常に重要な形式です。これにより、評価されないコードを書くことができます。例えば

```
(quote (+ 1 1))
; => 
(+ 1 1)
```

`(+ 1 1)` は `quote` の中にあるので、それを評価して `2` を返すことはしませんでした。 その代わり、式そのものを返します。これは引用符をサポートしない言語から来た人には奇妙に思えるかもしれませんが、Lispでは常に使用されている方法です。実際、そのためのショートカットがあります。

```
'(+ 1 1) 
```

これを読み込むと、次のように変換されます。

```
(quote (+ 1 1))
```

ありがたいことに、edn の `read-string` がこれを代行してくれます。 🙂

## クォートの評価

では、どのようにしてクォートを評価するのでしょうか。まず、クォートを検出する関数を作ってみましょう。

```
(defn seq-starts-with? [starts-with form]
  (and (seqable? form) (= (first form) starts-with)))

(def quote? (partial seq-starts-with? 'quote))
```

フォームがリストで、最初の要素がシンボル `quote` であるかどうかを確認します。

```
(quote? (read-string "'(+ 1 1)"))
; => 
true
```

魅力的なように動作します。では、`eval`を更新してみましょう。

```
(defn eval [form]
  (cond
    (literal? form) form
    (quote? form) (second form)))
```

フォームが `quote` であれば、その「second」部分を返します。そして出来上がり。

```
(eval (read-string "(quote (+ 1 1))"))
; => (+ 1 1)
```

## シンボルの紹介

次にシンボルについて説明します。シンボルはLispの特殊なデータ型です。変数と同じで、シンボルを評価するときに、それを調べるという意味です。



![image](/api/image/aHR0cHM6Ly91c2VyLWltYWdlcy5naXRodWJ1c2VyY29udGVudC5jb20vOTg0NTc0LzE1MDY1NTc2MS01YmQ2ODhlOS00ZDEzLTQ3ZjMtYTg4My1lYTZhMTdlYWRjNjEucG5n){style="max-width:100%"}


どこで調べるんだ？インタープリタにはある種の `environment` が必要で、それは私たちが定義したすべての変数を追跡します。例えば、`fav-num` というシンボルが `41` を指しているような環境があった場合、`fav-num` を評価すると次のようになります。

```
(eval env (read-string "fav-num"))
; => 
41
```

## シンボルを評価する

まず、この `environment` を作成しましょう。 シンボルとその値の対応関係を保持するために、java の `Hashmap` を利用することができる。

java の Hashmap をインポートしよう。

```
(ns simple-lisp.core
  (:refer-clojure :exclude [eval read read-string])
  (:require [clojure.edn :refer [read read-string]])
  (:import (java.util HashMap)))
```

そして、`env` マップを作成するための簡単な関数を作成します。

```
(defn env [] {:globe (HashMap. {'fav-num 41})})
```

これで `eval` で `env` マップを受け取り、シンボルを扱うことができるようになりました。

```
(defn eval [env form]
  (cond
    (literal? form) form
    (quote? form) (second form)
    (symbol? form) (lookup-symbol env form)))
```

Clojureにはすでに便利な `symbol?` という関数があります。それがtrueなら、 `lookup-symbol` とします。 以下は `lookup-symbol` がどのように見えるかです。

```
(defn lookup-symbol [{:keys [globe]} sym]
  (let [v (when (contains? globe sym) [(get globe sym)])]
    (assert v (format "expected value for sym = %s" sym))
    (first v)))
```

そして、シンボルが機能する!

```
(eval (env) (read-string "fav-num"))
; => 
41
```

## プリミティブの評価

次は、`(+ 1 1)` を動作させるための重要なステップです。私たちは `+` が何かを意味するようにしたいと思うでしょう。シンボル `+` は何を指すべきでしょうか？

Clojureの関数はどうでしょうか？

```
(defn env [] {:globe (HashMap. {'+ + 'fav-num 41})})
```

そうすると、今度はシンボル `+` を評価するたびに clojure の関数が返されるようになります。

```
(eval (env) (read-string "+"))
; => #object[clojure.core$_PLUS_ 0x4efcb4b5 "[email protected]"]
```

また、evalでclojureの関数を「受け取った」場合、それをリテラルとして扱えばいいだけです。

```
(def literal?
  (some-fn string? number? boolean? char? nil? fn?))
```

```
(eval (env) +)
; => #object[clojure.core$_PLUS_ 0x4efcb4b5 "[email protected]"]
```

これは私たちのエッセイでは小さな一歩ですが、私たちのLispでは大きな一歩です。 これで `(+ 1 1)` のすべての要素が評価できるようになりました。

## 原始的なアプリケーション

では、これらの関数をどのように実行すればよいのでしょうか？では、 `eval` を更新して、 `(+ 1 1)` のようなリストを扱えるようにしましょう。

```
(defn eval [env form]
  (cond
    (literal? form) form
    (quote? form) (second form)
    (symbol? form) (lookup-symbol env form)
    :else (eval-application env form)))
```

簡単だ。もし `form` が何であるか理解できなければ、それはリストであり、それを実行したいことを意味するに違いありません! これは `eval-application` がどのように動作するかを示しています。

```
(defn eval-application [env [f & args]]
  (let [f-evaled (eval env f)]
    (cond
      (fn? f-evaled) (apply f-evaled (eval-many env args)))))
```

`eval-application` では、リストの *最初の* 部分を評価します。この例では、シンボル `+` が評価され、clojure関数が返されます。

もし `f-evaled` が clojure の関数であれば(実際そうです)、次のように実行されます。

```
(apply f-evaled (eval-many env args))
```

`eval-many` は、フォームのリストを評価するための単なるヘルパーです。

```
(defn eval-many [e forms] (map (partial eval e) forms))
```

`args` は `(1 1)` となるので、 `eval-many` は `(1 1)` を返すことになります。

```
(apply + '(1 1))
```

すると、`2`が返され、`(+ 1 1)`が動作するようになります!

```
(eval (env) (read-string "(+ 1 1)")
; => 
2
```

再帰的に評価するので、すでに複雑なことができることを忘れないでください。

```
(eval (env) (read-string "(+ 1 (+ 1 fav-num))")
; => 
43
```

## def の紹介

OKY DOKE, 我々は環境を手に入れ、その環境から何かを探し出すシンボルを手に入れ、さらに `(+ 1 (+ 1 fav-num))` を評価することができるようになりました。

しかし、どうやって変数を定義するのでしょうか？例えば、`second-fav`というシンボルが`42`を指すようにしたいとしましょう。

そうすると、特殊な形式を導入することができます。このようなものです。

```
(def second-fav (+ 1 fav-num))
```

この `def` フォームを受け取ったら、環境を更新して、シンボル `second-fav` を `(+ 1 fav-num)` の評価値がどうなるかに向けるだけでいいのです。

## Defを評価する

という感じですね。まずはこのフォームを検出してみましょう。

```
(def def? (partial seq-starts-with? 'def))
```

そして、それを処理するために `eval` を更新します。

```
(defn eval [env form]
  (cond
    (literal? form) form
    (quote? form) (second form)
    (symbol? form) (lookup-symbol env form)
    (def? form) (eval-def env form)
    :else (eval-application env form)))
```

そして、`eval-def`が必要とすることは、これだけです。

```
(defn eval-def [env [_ k v]]
  (assert (symbol? k) (format "expected k = %s to be a symbol" k))
  (.put (:globe env) k (eval env v)))
```

ここでは、`def` の最初の引数がシンボル `k` であることが分かっています。 2 番目の引数は保存したい *値* `v` です。そこで、環境の `globe` ハッシュマップを更新して、シンボル `k` を `(eval env v)` に指定します。この場合、 `k` は `second-fav` で、 `v` は `(+ 1 fav-num)` となり、 `(eval env v)` は `42` となります。

かっこいいですね、これは。

```
(let [e (env)] 
    (eval e (read-string "(def second-fav (+ 1 fav-num))"))
    (eval e (read-string "second-fav")))
; => 
42
```

## ifの導入

さて、もう一つ特殊な形式を実装してみましょう。条件付きロジックを処理するものが必要だ。

```
(if (= fav-num 41) 'yess 'noo))
```

`if` を得ると、テストフォーム`(= fav-num 41)`を評価することができます。もしそれが真なら、真のケース `'yess` を評価します。 そうでなければ、偽のケースを評価することになります。`'noo` となります。 計画的ですね。

## ifの評価

ifを実装するために、いつものようにまず検出しましょう。

```
(def if? (partial seq-starts-with? 'if))
```

そして、それを `eval` で使用します。

```
(defn eval [env form]
  (cond
    (literal? form) form
    (quote? form) (second form)
    (symbol? form) (lookup-symbol env form)
    (def? form) (eval-def env form)
    (if? form) (eval-if env form)
    :else (eval-application env form)))
```

そして、`eval-if`はこんな感じでしょうか。

```
(defn eval-if [env [_ test-form when-true when-false]]
  (let [evaled-test (eval env test-form)]
    (eval env (if evaled-test when-true when-false))))
```

ほんの数行、私たちの説明に忠実に従ったものです。`evaled-test`が真であるかどうかを確認します。そして、 `when-true` か `when-false` のどちらかを評価する。

ついでに、 `env` に素敵なプリミティブ関数をたくさん追加してみよう。

```
(defn env [] {:globe (HashMap. {'+ + '= = 'list list 'map map 'concat concat
                                'first first 'second second 'not not 'fav-num 41})})
```

そして、バーン。この小さなインタプリタがかなり多くのことをできるようになったのです

```
    (eval (env) (read-string "(if (= fav-num 41) 'yess 'noo)"))
    ; => yess
```

## 関数の紹介

さらに上を目指しましょう。Clojureの `+` などはありますが、プログラマが *自分自身の* 関数を書きたいとしたらどうでしょうか？ここに例があります。

```
((clo (x) (+ x fav-num) 2))
```

ここで、プログラマは `x` を受け取り、それに `fav-num` を加える関数を書いたとします。

彼らがこう書いたとします。彼らの関数を実行するにはどうしたらいいでしょうか？さて、彼らの定義には `arguments` と `body` があります。 この場合、それぞれ `(x)` と `(+ x fav-num)` です。この例では、関数は `2` で呼び出されます。

もし、 `(+ x fav-num)` を評価することができれば、 `x` が `2` を指す環境で、まるで関数を *ran* したかのようになるのです! このアイデアを説明するための図があります。


![image](/api/image/aHR0cHM6Ly91c2VyLWltYWdlcy5naXRodWJ1c2VyY29udGVudC5jb20vOTg0NTc0LzE1MDY1NTc2OC00MzBkN2ZjMC05MDQyLTQ0ZGUtYmYyOS0zNjM4ZjA5YTllYzkucG5n){style="max-width:100%"}


どうやって `x` に `2` を指定するのでしょうか？ なぜなら、`body` の評価が終わった後でも、`x` は *永久に* `2` に設定されてしまうからです。

だから、新しいアイディアが必要なんだ。スコープ`です。 スコープ`は環境として考えることができ、そこでは `body` が評価されている間だけ変更が持続する。このような概念を導入することができれば、関数ができることになる。

## 関数の評価

さて、これを動作させましょう。まず、誰かが `(clo (x) (+ x 1))` と書いたら、実際には `eval-application` をしようとしないことを確認しましょう。 その代わり、これを新しい `closure` リテラルとして扱います。

それを検出することができます。

```
(def closure? (partial seq-starts-with? 'clo))
```

そして、私たちの `literal?` を更新してください。

```
(def literal?
  (some-fn string? number? boolean? char? nil? fn? closure?))
```

今、ユーザーが関数を書いただけでは、それ自体を返してしまいます。

```
(eval (env) (read-string "(clo (x) (+ x fav-num))"))
; => (clo (x) (+ x fav-num))
```

次に、`環境`を更新して、新しい`scope`マップを持つようにします。

```
(defn env [] {:globe (HashMap. {'+ + '= = 'list list 'map map 'concat concat
                                'first first 'second second 'fav-num 41})
              :scope {}})
```

そして、`lookup-symbol` を更新して、スコープ内の変数 *も* 探すようにしましょう。

```
(defn lookup-symbol [{:keys [globe scope]} sym]
  (let [v (some (fn [m] (when (contains? m sym) [(get m sym)]))
                [globe scope])]
    (assert v (format "expected value for sym = %s" sym))
    (first v)))
```

もっと近くに、もっと近くに。さあ、実行させましょう。こう書けば

```
((clo (x) (+ x fav-num)) 1)
```

このリストは `eval-application` に行くことになるでしょう。 更新してみましょう。

```
(defn eval-application [env [f & args]]
  (let [f-evaled (eval env f)]
    (cond
      (fn? f-evaled) (apply f-evaled (eval-many env args))
      (closure? f-evaled) (eval-closure env f-evaled (eval-many env args)))))
```

`f-evaled` は `(clo (x) (+ x fav-num))` となります。 これは `(closure? f-evaled)` が真になることを意味します。

`(eval-many env args)`ですべての引数を評価すると `(1)` となり、これを `eval-closure` に渡します。 以下は `eval-closure` がどのように見えるかです。

```
(defn eval-closure [env [_ syms body] args]
  (eval (assoc env :scope (assign-vars syms args)) body))
```

図と同じようにします。そして `syms` は `(x)` となり、`args` は `(1)` となります。 そして、新しいスコープを作成します。

```
(defn assign-vars [syms args]
  (assert (= (count syms) (count args))
          (format "syms and args must match syms: %s args: %s"
                  (vec syms) (vec args)))
  (into {} (map vector syms args)))
```

これは `{x 2}` です。 これを環境にマージして、`body`を評価します。

突然ですが、`(+ x fav-num)` は `43` になります!

```
(eval (env) (read-string "((clo (x) (+ x fav-num)) 2)"))
; => 43
```

## レキシカル・スコープを導入する

多くの言語がレキシカル・スコープをサポートしている。例えば、javascriptでは以下のようなことができます。

```
const makeAdder = (n) => (a) => n + a
const addTwo = makeAdder(2);
addTwo(4) 
// => 
6
```

ここで、 `makeAdder` は関数を返します。この関数は `n` の値を *定義されている場所* で記憶していました。これを私たちの言語で実現するにはどうしたらいいのでしょうか？

さて、まず最初に、プログラマに現在のスコープを見せるようにするとしよう。以下はその例です。

```
((clo (x) scope) 2)
; => 
{x 2}
```

この特別な `scope` シンボルは、評価されると `{x 2}` を返し、 *current* スコープになります!

さて、関数リテラルを更新してみるとどうでしょう？次のような感じでしょうか。

```
(clo {n 2} (a) (+ n a))
```

ここでは、`scope`のための特別な場所があります!  関数を定義するときに、その関数が定義されている場所のスコープをそこに「ポン」と置くことができれば、badabing badaboom, lexical scoping をサポートすることになります!

ここで、 `make-adder` がどのように動作するのか見てみましょう。

```
(def make-adder (clo nil (n) 
                  (list 'clo scope '(y) '(+ y n)))) 
(def add-two (make-adder 2))
(add-two 4)
; => 
6
```

`(make-adder 2)`を実行すると、`(list 'clo scope '(y) '(+ y n))` を評価することになります。 これは `(clo {n 2} (y) (+ y n))` を返すでしょう!  突然ですが、私たちはレキシカルスコープをサポートしています。

`(list 'clo scope '(y) '(+ y n)))` --- と書くのは変だと思うかもしれません。でもね、もうすぐマクロがサポートされるから、そのおかげでこの書き方がぴったりになるんだよ。待っててね 🙂.

## Implementing Lexical Scope

Okay. that's a lot of explaining, but the changes will be simple. First we need to make sure that if we get a `scope` symbol, we return the current scope:

```
(defn lookup-symbol [{:keys [globe scope]} sym]
  (let [v (some (fn [m] (when (contains? m sym) [(get m sym)]))
                [{'scope scope} globe scope])]
    (assert v (format "expected value for sym = %s" sym))
    (first v)))
```

And now all we need to do, is to update `eval-closure`:

```
(defn eval-closure [env [_ scope syms body] args]
  (eval (assoc env :scope (merge scope (assign-vars syms args))) body))
```

Here we take the `scope` *from* the closure itself! With that, lexical scoping works!

```
(((clo nil (x)
        (list 'clo scope '(y) '(+ y x))) 2) 4)
; => 
6
```

## Introducing Macros

Now, we have all we need to implement macros! Let's think about what macros really are.

Imagine writing the function `unless`:

```
(def unless (clo nil (test v)
              (if (not test) v)))
```

Will it work? If we ran `(unless (= fav-num 41) 'yes)`, it *would* return the symbol `yes`.

But what if we ran this?

```
(unless (= fav-num 41) (throw-error))
```

Since `unless` is a function, we would evaluate each argument first right? In that case, we'd evaluate `(= fav-num 41)`, which would be `true`.  But, we'd *also* evaluate `(throw-error)`.  That would break our program. This defeats the whole purpose of `unless` , as `(throw-error)` was supposed to run *only* when `test` was false.



![image](/api/image/aHR0cHM6Ly91c2VyLWltYWdlcy5naXRodWJ1c2VyY29udGVudC5jb20vOTg0NTc0LzE1MDY1NTc3Mi1hNTlmNGJkOC1mNDc1LTQxMGMtODFmYi02YWI2NjlhNDY1OTEucG5n){style="max-width:100%"}


Now, imagine we wrote the *macro* unless:

```
(def unless (mac (clo nil (test v)
                    (list 'if (list 'not test) v))))
```

If we ran `(unless (= fav-num 41) (throw-error))`, here's what would happen:

The value of `test` would not be `true`, it would actually be the list `(= fav-num 41)`.  Similarly, we wouldn't evaluate `(throw-error)` .  `v` would just be the actual list `(throw-error)`.

*The arguments to a macro are not evaluated.* When the macro runs, it returns *code.* The result of

```
(list 'if (list 'not test) v))
```

would become

```
(if (not (= fav-num 41)) (throw-error))
```

And when we evaluated *that,* things would work as expected!  `(throw-error)` would never evaluate.

This is the key difference between functions and macros. Functions take *evaluated values* as arguments and return evaluated values. Macros receive *unevaluated* arguments, return *code, and that code is evaluated.*


![image](/api/image/aHR0cHM6Ly91c2VyLWltYWdlcy5naXRodWJ1c2VyY29udGVudC5jb20vOTg0NTc0LzE1MDY1NTc3OS1mMDMxNWQyNS1lZjZlLTQzYjktOTBiNy1jMmYyMDE2YTkyOGIucG5n){style="max-width:100%"}


An important piece to note is that we eval *twice.* We give the macro the unevaluated arguments, which returns new code, and we once again *evaluate* that.

So, how can we support this?

## Evaluating Macros

So much explanation, for such little code. Are you ready? Let's add macros.

First, let's use this new structure for a macro: a macro is a list that begins with `mac`, and has closure inside:

```
(mac (clo nil (...))
```

We can detect macro, and mark it as a literal:

```
(def macro? (partial seq-starts-with? 'mac))
    
(def literal?
  (some-fn string? number? boolean? char? nil? fn? closure? macro?))
```

Next, we'll want to update `eval-application`:

```
(defn eval-application [env [f & args]]
  (let [f-evaled (eval env f)]
    (cond
      (fn? f-evaled) (apply f-evaled (eval-many env args))
      (closure? f-evaled) (eval-closure env f-evaled (eval-many env args))
      (macro? f-evaled) (eval-macro env f-evaled args))))
```

Before we always ran `(eval-many env args)`.  But this time, if it's a macro, we pass in `args` directly! That's the "code" itself 🙂.

And now for `eval-macro`:

```
(defn eval-macro [env [_ clo] args]
  (eval env
        (eval env (concat [clo] (map (partial list 'quote) args)))))
```

Oh my god. 3 lines!! We do exactly as we said in our diagram:



![image](/api/image/aHR0cHM6Ly91c2VyLWltYWdlcy5naXRodWJ1c2VyY29udGVudC5jb20vOTg0NTc0LzE1MDY1NTc5MC02MWFhOTQ5OC05NDgzLTQ4ODYtOGRiMC1lYmM3NzE1OTM2MzkucG5n){style="max-width:100%"}


We take the "closure" out of our macro, and *run* it with *unevaluated* args. We can do that just by wrapping each `arg` in a quote: `(map (partial list 'quote) args)`.

Once we have the resulting code, we *evaluate* that again, and boom, we have macros.

```
(def unless (mac (clo nil (test v)
                    (list 'if (list 'not test) v))))
; works!
(unless (= fav-num 41) (throw-error))
```

# Your own Macros

Okay, we have a Lisp that supports macros. Let's go ahead and write some of our own!

## defmacro

Let's get meta off the bat. Notice what we do when we define a macro:

```
(def unless (mac (clo nil (test v)
                      (list 'if (list 'not test) v))))
```

This whole `(mac (clo nil …))` business is a bit unnecessary. Let's just write macro that does this for us!

```
(def defmacro
      (mac (clo nil (n p e)
                (list 'def n
                      (list 'mac (list 'clo nil p e))))))
```

This generates the code `(def n (mac (clo nil …)))`.  Now we could write:

```
(defmacro unless (test v) 
  (list 'if (list 'not test) v))
```

Cool!

## fn

Okay, remember how we wrote our function for lexical scoping?

```
(def make-adder (clo nil (n)
                      (list 'clo scope '(y) '(+ y n))))
```

Let's have a macro write this for us:

```
(defmacro fn (args body)
              (list 'list ''clo 'scope
                    (list 'quote args)
                    (list 'quote body)))
```

Here's what happens if we wrote:

```
(def make-adder (clo nil (n)
                    (fn (y) (+ y n))))

(def add-two (make-adder 2))
```

When the macro expansion for `fn` runs, the `args` would would be `(y)` and `body` `(+ y n)`.  So

```
(list 'list ''clo 'scope
      (list 'quote args)
      (list 'quote body))
```

would expand to

```
(list 'clo scope '(y) '(+ y n))
```

and that's *exactly* what we wrote by hand! Bam, now we can use `fn` instead of this whole `'clo scope` business.

## defn

Now if we wanted to define functions, we could write:

```
(def add-fav-num (fn (x) (+ x fav-num)))
```

But we can make it tighter. Let's add a `defn` macro, so we could write:

```
(defn add-fav-num (x) (+ x fav-num))
```

Easy peasy:

```
(defmacro defn (n args body)
  (list 'def n (list 'fn args body)))
```

## let

One more cool macro. Right now, there's no real way to define temporary variables. Something like:

```
(let ((x 1) (y 2)) 
  (+ x y))
; => 
3
```

How could we support it? Well, what if we *rewrote* `(let …)` to:

```
((fn (x y) (+ x y)) 1 2)
```

We could just use the argument list of a function to create these variables! Perfecto. Here's the macro:

```
(defmacro let (pairs body)
  (concat (list (list 'fn (map first pairs) body))
                (map second pairs)))
```

# Fin

What a journey. 80 lines, 4000 words, and hopefully a fun time 🙂. You now have a Lisp with macros, and you've written some cool ones yourself.  Here's the [full source](https://github.com/stopachka/llisp/blob/main/src/llisp/core.clj){style="text-decoration:underline" target="_blank"} if you'd like to follow along from there.

**Credits**

The idea to represent macros and closures like this, came from PG's [Bel](http://www.paulgraham.com/bel.html){style="text-decoration:underline" target="_blank"}.

*Thanks to Mark Shlick, Daniel Woelfel for reviewing drafts of this essay.*


------------------------------------------------------------------------

#### *Thoughts? Reach out to me via twitter or email : )*







