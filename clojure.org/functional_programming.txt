■ http://clojure.org/functional_programming

Functional Programming	→	関数型プログラミング
Clojure is a functional programming language.	→	Clojureは、関数型言語です。
It provides the tools to avoid mutable state, provides functions as first-class objects, and emphasizes recursive iteration instead of side-effect based looping.	→	それは変わりやすい州を避けるためのツールを提供して、第一級物として機能を提供して、副作用ベースのルーピングの代わりに再帰的な繰り返しを強調します。
Clojure is impure, in that it doesn't force your program to be referentially transparent, and doesn't strive for 'provable' programs.	→	Clojureは、それがあなたのプログラムに関連してトランスペアレントなことを強いないという点で汚くて、『証明できる』プログラムのために努力しません。
The philosophy behind Clojure is that most parts of most programs should be functional, and that programs that are more functional are more robust.	→	Clojureの後の哲学は大部分のプログラムの大半が機能的でなければならないということです、そして、より機能的であるそのプログラムはより強力です。
First-class functionsfn creates a function object.	→	第一級機能fnは、機能物をつくります。
It yields a value like any other - you can store it in a var, pass it to functions etc	→	それは他のもののような価値を与えます－あなたはそれを1バールに保管することができて、機能その他にそれを通過します。

(def hello (fn [] "Hello world"))
-> #'user/hello
(hello)
-> "Hello world"

defn is a macro that makes defining functions a little simpler.	→	defnは、機能を定めることを少しより単純にするマクロです。
Clojure supports arity overloading in a single function object, self-reference, and variable-arity functions using &:	→	arityに一つの機能で物（自己言及）にオーバーロードするClojure支持物、そして、可変的なarity機能使うこと∥と：

;trumped-up example
(defn argcount
  ([] 0)
  ([x] 1)
  ([x y] 2)
  ([x y & more] (+ (argcount x y) (count more))))
-> #'user/argcount
(argcount)
-> 0
(argcount 1)
-> 1
(argcount 1 2)
-> 2
(argcount 1 2 3 4 5)
-> 5


You can create local names for values inside a function using let.	→	あなたは、使用することがした機能の中に、価格のローカル名前をつくることができます。
The scope of any local names is lexical, so a function created in the scope of local names will close over their values:	→	どんなローカル名前の範囲でも語彙的であるので、ローカル名前の範囲で作成される機能は彼らの値以上閉まります：

(defn make-adder [x]
  (let [y x]
    (fn [z] (+ y z))))
(def add2 (make-adder 2))
(add2 4)
-> 6

Locals created with let are not variables.	→	そうさせられて創造される地方住民は、変数でありません。
Once created their values never change!	→	一度、つくられて彼らの価格は、決して変わりません！
Immutable Data Structures	→	不変のデータ構造
The easiest way to avoid mutating state is to use immutable data structures.	→	州を変異させることを避ける最も簡単な方法は、不変のデータ構造を使用することです。
Clojure provides a set of immutable lists, vectors, sets and maps.	→	Clojureは、一組の不変のリスト、ベクトル、セットと地図を提供します。
Since they can't be changed, 'adding' or 'removing' something from an immutable collection means creating a new collection just like the old one but with the needed change.	→	彼らが変わることができないので、何かを不変のコレクションから『加える』か、『取り除く』ことは必要とされた変化でまるで古いもののような新しいコレクションを以外作成することを意味します。
Persistence is a term used to describe the property wherein the old version of the collection is still available after the 'change', and that the collection maintains its performance guarantees for most operations.	→	持続は、コレクションの古いバージョンがまだ『変化』の後利用できる、そして、コレクションが大部分の活動に対するそのパフォーマンス保証を維持する資産を記述するのに用いられる語です。
Specifically, this means that the new version can't be created using a full copy, since that would require linear time.	→	具体的には、それが線形時間を必要とする時から、これは、新しいバージョンが完全なコピーを使って作成されることができないことを意味します。
Inevitably, persistent collections are implemented using linked data structures, so that the new versions can share structure with the prior version.	→	必然的に、持続的なコレクションは結ばれたデータ構造を使用して実行されます、そのため、新しいバージョンは構造を先のバージョンと共有することができます。
Singly-linked lists and trees are the basic functional data structures, to which Clojure adds a hash map, set and vector both based upon array mapped hash tries.	→	単独でにリンクされたリストと木は、基本的な機能的なデータ構造（両方とも、図にされたハッシュがためす配列に基づくハッシュ・マップ、セットとベクトルを、Clojureは加えます）です。
The collections have readable representations and common interfaces:	→	コレクションには、読み込み可能な表現と一般のインターフェースがあります：

(let [my-vector [1 2 3 4]
      my-map {:fred "ethel"}
      my-list (list 4 3 2 1)]
  (list
    (conj my-vector 5)
    (assoc my-map :ricky "lucy")
    (conj my-list 5)
    ;the originals are intact
    my-vector
    my-map
    my-list))
-> ([1 2 3 4 5] {:ricky "lucy", :fred "ethel"} (5 4 3 2 1) [1 2 3 4] {:fred "ethel"} (4 3 2 1))

Applications often need to associate attributes and other data about data that is orthogonal to the logical value of the data.	→	アプリケーションは、しばしば、データの論理的値に対して直角であるデータについて特質と他のデータを結びつける必要があります。
Clojure provides direct support for this metadata.	→	Clojureは、直接の支持をこのメタデータに提供します。
Symbols, and all of the collections, support a metadata map.	→	シンボル（本当にコレクションの）は、メタデータ・マップを支えます。
It can be accessed with the meta function.	→	それは、メタ機能でアクセスされることができます。
Metadata does not impact equality semantics, nor will metadata be seen in operations on the value of a collection.	→	メタデータは平等意味論に影響を与えませんし、メタデータはコレクションの価値に関して活動において見られません。
Metadata can be read, and can be printed	→	メタデータは読まれることができて、印刷されることができます。

(def v [1 2 3])
(def attributed-v (with-meta v {:source :trusted}))
(:source (meta attributed-v))
-> :trusted
(= v attributed-v)
-> true

Extensible Abstractions	→	伸長可能な抽象概念
Clojure uses Java interfaces to define its core data structures.	→	Clojureは、その中心的なデータ構造を定めるために、Javaインターフェースを使用します。
This allows for extensions of Clojure to new concrete implementations of these interfaces, and the library functions will work with these extensions.	→	これはこれらのインターフェースの新しい具体的な機能にClojureの拡張を考慮に入れます、そして、図書館機能はこれらの機能拡張で動作します。
This is a big improvement vs. hardwiring a language to the concrete implementations of its data types.	→	これは、そのデータの具体的な実現例への言語がタイプする大きい改善対コンピュータ内部の電子部品間の結線接続です。
A good example of this is the seq interface.	→	これの良い例は、seqインターフェースです。
By making the core Lisp list construct into an abstraction, a wealth of library functions are extended to any data structure that can provide a sequential interface to its contents.	→	中心的なLISPリスト構成概念を抽象概念にすることによって、連続したインターフェースをその内容に提供することができるどんなデータ構造にでも、豊かな図書館機能は応用されます。
All of the Clojure data structures can provide seqs.	→	Clojureデータ構造の全ては、seqsを提供することができます。
Seqs can be used like iterators or generators in other languages, with the significant advantage that seqs are immutable and persistent.	→	seqsが不変で持続的である有意な有利な条件で、Seqsが他の言語で反復子または発電機のように使われることができます。
Seqs are extremely simple, providing a first function, which return the first item in the sequence, and a rest function which returns the rest of the sequence, which is itself either a seq or nil	→	Seqsはとても単純です。そして、残りのシーケンスを返す最初の機能（それはシーケンスぶりのアイテムを返します）と休み機能を提供します。そして、それはそれ自体seqか無です。

(let [my-vector [1 2 3 4]
      my-map {:fred "ethel" :ricky "lucy"}
      my-list (list 4 3 2 1)]
  [(first my-vector)
   (rest my-vector)
   (keys my-map)
   (vals my-map)
   (first my-list)
   (rest my-list)])
-> [1 (2 3 4) (:ricky :fred) ("lucy" "ethel") 4 (3 2 1)]

Clojure図書館機能の多くは、ゆったりとseqsを生じて、消費します：

;サイクルは、『無限』seqを生じます！
;cycle produces an 'infinite' seq!
(take 15 (cycle [1 2 3 4]))
-> (1 2 3 4 1 2 3 4 1 2 3 4 1 2 3)

You can define your own lazy seq-producing functions using the lazy-seq macro, which takes a body of expressions that will be called on demand to produce a list of 0 or more items. Here's a simplified take:

(defn take [n coll]
  (lazy-seq
    (when (pos? n)
      (when-let [s (seq coll)]
       (cons (first s) (take (dec n) (rest s)))))))


Recursive Looping	→	再帰的なルーピング
In the absence of mutable local variables, looping and iteration must take a different form than in languages with built-in for or while constructs that are controlled by changing state.	→	変わりやすい局所変数がない場合、ルーピングと繰り返しは、作り付け家具による言語のより異なる形をとらなければなりませんまたは、その一方で∥州を変えることによってコントロールされる構成概念。
In functional languages looping and iteration are replaced / implemented via recursive function calls.	→	関数型言語では、ルーピングと繰り返しは、とって代わられて/再帰的な機能呼び出しを通して実行されます。
Many such languages guarantee that function calls made in tail position do not consume stack space, and thus recursive loops utilize constant space.	→	そのような言語が尾位置でなされるその機能呼び出しに保証する多くはスタック・スペースを消費しません、そして、このように、再帰的ループは恒常的なスペースを利用します。
Since Clojure uses the Java calling conventions, it cannot, and does not, make the same tail call optimization guarantees.	→	Clojureが大会を招集しているJavaを使用するので、それは同じ尾を最適化を保証と呼ばせることができなくて、ません。
Instead, it provides the recur special operator, which does constant-space recursive looping by rebinding and jumping to the nearest enclosing loop or function frame.	→	その代わりに、それは備えをします繰り返されます特別なオペレーター（最も近い囲んでいるループまたは機能フレームを再結合することとジャンプによって、一定にスペース再帰的なルーピングをします）。
While not as general as tail-call-optimization, it allows most of the same elegant constructs, and offers the advantage of checking that calls to recur can only happen in a tail position	→	尾-呼び出し-最適化ほど一般的でない間、それは大部分の同じエレガントな構成概念を許して、繰り返されるという要求が尾位置で起こることができるだけのことを確認することの長所を提供します。


(defn my-zipmap [keys vals]
  (loop [my-map {}
         my-keys (seq keys)
         my-vals (seq vals)]
    (if (and my-keys my-vals)
      (recur (assoc my-map (first my-keys) (first my-vals))
             (next my-keys)
             (next my-vals))
      my-map)))
(my-zipmap [:a :b :c] [1 2 3])
-> {:b 2, :c 3, :a 1}


相互再帰が要求される状況のために、繰り返されます使われることができません。
その代わりに、トランポリンは良いオプションである場合があります。

