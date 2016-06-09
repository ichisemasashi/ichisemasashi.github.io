---
layout: post
title: Hy言語チュートリアルを触ってみる(2)
date:   2016-06-09 11:02:07 +0900
categories: hy-lang
---
# [Hy言語チュートリアル](http://docs.hylang.org/en/stable/tutorial.html)を触ってみる(2)

## ループ処理があるらしい。

```
=> (for [i (range 10)]
...   (print (+ "'i' is now at " (str i))))
'i' is now at 0
'i' is now at 1
'i' is now at 2
'i' is now at 3
'i' is now at 4
'i' is now at 5
'i' is now at 6
'i' is now at 7
'i' is now at 8
'i' is now at 9
```

`(range 10)`が何なのか気になる。

```
=> (range 10)
xrange(10)
```

'xrange'って何だろう。

「[Python 標準ライブラリ » 2. 組み込み関数](http://docs.python.jp/2/library/functions.html)」より
> xrange(stop)
>
> xrange(start, stop[, step])
>
> この関数は range() に非常によく似ていますが、リストの代わりに XRange 型 を返します。このオブジェクトは不透明なシーケンス型で、対応するリストと同じ値を持ちますが、それらの値全てを同時に記憶しません。 ragne() に対する xrange() の利点は微々たるものです (xrange() は要求に応じて値を生成するからです) ただし、メモリ量の厳しい計算機で巨大な範囲の値を使う時や、(ループがよく break で中断されるといったように) 範囲中の全ての値を使うとは限らない場合はその限りではありません。xrange オブジェクトについてのさらに詳しい情報については、 XRange 型 と シーケンス型 — str, unicode, list, tuple, bytearray, buffer, xrange を参照して下さい。

どうやら、遅延の効果があるみたいです。
さらに、「[5.6.3. XRange 型](http://docs.python.jp/2/library/stdtypes.html#typesseq-xrange)」より
>xrange 型は値の変更不能なシーケンスで、広範なループ処理に使われています。 xrange 型の利点は、 xrange オブジェクトは表現する値域の大きさにかかわらず常に同じ量のメモリしか占めないということです。はっきりしたパフォーマンス上の利点はありません。
>
>XRange オブジェクトは非常に限られた振る舞い、すなわち、インデクス検索、反復、 len() 関数のみをサポートしています。

`(range 10)`の`range`はpythonのrange()とは違うみたいです。

```
>>> range(10)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> range(1, 11)
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> range(0, 30, 5)
[0, 5, 10, 15, 20, 25]
>>> range(0, 10, 3)
[0, 3, 6, 9]
>>> range(0, -10, -1)
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
>>> range(0)
[]
>>> range(1, 0)
[]
```

さまざまなpythonライブラリを利用できます。

```
=> (import os)
=>
=> (if (os.path.isdir "/tmp/somedir")
...   (os.mkdir "/tmp/somedir/anotherdir")
...   (print "Hey, that path isn't there!"))
Hey, that path isn't there!
```

pythonのコンテキストマネージャーも利用できる。

```
=> (with [[f (open "/tmp/data.in")]]
...   (print (.read f)))
Traceback (most recent call last):
  File "<input>", line 1, in <module>
IOError: [Errno 2] No such file or directory: u'/tmp/data.in'
=> (with [[f (open "../README.md")]]                                         
...   (print (.read f)))

     ,-----.,--.                  ,--. ,---.   ,--.,------.  ,------.
    '  .--./|  | ,---. ,--.,--. ,-|  || o   \  |  ||  .-.  \ |  .---'
    |  |    |  || .-. ||  ||  |' .-. |`..'  |  |  ||  |  \  :|  `--,
    '  '--'\|  |' '-' ''  ''  '\ `-' | .'  /   |  ||  '--'  /|  `---.
     `-----'`--' `---'  `----'  `---'  `--'    `--'`-------' `------'
    -----------------------------------------------------------------


Hi there! Welcome to Cloud9 IDE!

To get you started, create some files, play with the terminal,
or visit http://docs.c9.io for our documentation.
If you want, you can also go watch some training videos at
http://www.youtube.com/user/c9ide.

Happy coding!
The Cloud9 IDE team
```

[pythonのコンテキストマネージャー](http://docs.python.jp/2/reference/datamodel.html#context-managers)について
>コンテキストマネージャ(context manager) とは、 with 文の実行時にランタイムコンテキストを定義するオブジェクトです。コンテキストマネージャは、コードブロックを実行するために必要な入り口および出口の処理を扱います。コンテキストマネージャは通常、 with 文（ with 文 の章を参照）により起動されますが、これらのメソッドを直接呼び出すことで起動することもできます。

「[リストの内包表記 (list comprehension)](http://docs.python.jp/2/reference/expressions.html#index-13)」の例

```
=> (setv odds-squared
...   (list-comp
...     (pow num 2)
...     (num (range 100))
...     (= (% num 2) 1)))
=> odds-squared
[1L, 9L, 25L, 49L, 81L, 121L, 169L, 225L, 289L, 361L, 441L, 529L, 625L, 729L, 841L, 961L, 1089L, 1225L, 1369L, 1521L, 1681L, 1849L, 2025L, 2209L, 2401L, 2601L, 2809L, 3025L, 3249L, 3481L, 3721L, 3969L, 4225L, 4489L, 4761L, 5041L, 5329L, 5625L, 5929L, 6241L, 6561L, 6889L, 7225L, 7569L, 7921L, 8281L, 8649L, 9025L, 9409L, 9801L]
```

チュートリアルには、さらに以下の例があった。(たぶん、リストの内包表記についての説明。)

```
=> ; And, an example stolen shamelessly from a Clojure page:
=> ; Let's list all the blocks of a Chessboard:
=>
=> (list-comp
...   (, x y)
...   (x (range 8)
...    y "ABCDEFGH"))
[(0, u'A'), (0, u'B'), (0, u'C'), (0, u'D'), (0, u'E'), (0, u'F'), (0, u'G'), (0, u'H'), (1, u'A'), (1, u'B'), (1, u'C'), (1, u'D'), (1, u'E'), (1, u'F'), (1, u'G'), (1, u'H'), (2, u'A'), (2, u'B'), (2, u'C'), (2, u'D'), (2, u'E'), (2, u'F'), (2, u'G'), (2, u'H'), (3, u'A'), (3, u'B'), (3, u'C'), (3, u'D'), (3, u'E'), (3, u'F'), (3, u'G'), (3, u'H'), (4, u'A'), (4, u'B'), (4, u'C'), (4, u'D'), (4, u'E'), (4, u'F'), (4, u'G'), (4, u'H'), (5, u'A'), (5, u'B'), (5, u'C'), (5, u'D'), (5, u'E'), (5, u'F'), (5, u'G'), (5, u'H'), (6, u'A'), (6, u'B'), (6, u'C'), (6, u'D'), (6, u'E'), (6, u'F'), (6, u'G'), (6, u'H'), (7, u'A'), (7, u'B'), (7, u'C'), (7, u'D'), (7, u'E'), (7, u'F'), (7, u'G'), (7, u'H')]
```

pythonの仮引数やキーワード変数なども利用できるらしい。

```
=> (defn optional-arg [pos1 pos2 &optional keyword1 [keyword2 42]]
...   [pos1 pos2 keyword1 keyword2])
=>
=> (optional-arg 1 2)
[1L, 2L, None, 42L]
=> (optional-arg 1 2 3 4)
[1L, 2L, 3L, 4L]
=> (optional-arg :keyword1 1
...               :pos2 2
...               :pos1 3
...               :keyword2 4)
[3L, 2L, 1L, 4L]
```

数値に`L`が付くのがなんとなく気になります。

`apply`も使えます。

```
=> (setv args [1 2])
=> (setv kwargs {"keyword2" 3
...               "keyword1" 4})
=> (apply optional-arg args kwargs)
[1L, 2L, 4L, 3L]
```

辞書形式のキーワード引数の例

```
=> (defn another-style [&key {"key1" "val1" "key2" "val2"}]
...   [key1 key2])
=> (another-style {:key1 'a :key2 1})
[{u'\ufdd0:key2': 1L, u'\ufdd0:key1': u'a'}, u'val2']
=> (another-style {:pos1 0} {:key1 'b :key2 'c})                             
[{u'\ufdd0:pos1': 0L}, {u'\ufdd0:key2': u'c', u'\ufdd0:key1': u'b'}]
```

pythonの`*`, `**`引数をサポートしている例

```
=> (defn some-func [foo bar &rest args &kwargs kwargs]
...   (import pprint)
...   (pprint.pprint (, foo bar args kwargs)))
=> (some-func 1 2 3 4 5 6 7 8 9)
(1L, 2L, (3L, 4L, 5L, 6L, 7L, 8L, 9L), {})
=> (some-func 1 2 3 4 5 6 7 8 9 kwargs 'a)
(1L,
 2L,
 (3L, 4L, 5L, 6L, 7L, 8L, 9L, {u'keyword1': 4L, u'keyword2': 3L}, u'a'),
 {})
=> (some-func 1 2 3 4 5 6 7 8 9 :kwargs 'a)                                  
(1L, 2L, (3L, 4L, 5L, 6L, 7L, 8L, 9L), {'kwargs': u'a'})
=> (some-func 1 2 3 4 5 6 7 8 9 :kwargs '(a 1))
(1L, 2L, (3L, 4L, 5L, 6L, 7L, 8L, 9L), {'kwargs': (u'a' 1L)})
=> (some-func 1 2 3 4 5 6 7 8 9 :apple '(a 1))                               
(1L, 2L, (3L, 4L, 5L, 6L, 7L, 8L, 9L), {'apple': (u'a' 1L)})
```

「[呼び出し (call)](http://docs.python.jp/2/reference/expressions.html#calls)」より

> 仮引数スロットの数よりも多くの固定引数がある場合、構文 *identifier を使って指定された仮引数がないかぎり、 TypeError 例外が送出されます; 仮引数 *identifier がある場合、この仮引数は余分な固定引数が入ったタプル (もしくは、余分な固定引数がない場合には空のタプル) を受け取ります。

> キーワード引数のいずれかが仮引数名に対応しない場合、構文 **identifier を使って指定された仮引数がない限り、 TypeError 例外が送出されます; 仮引数 **identifier がある場合、この仮引数は余分なキーワード引数が入った (キーワードをキーとし、引数値をキーに対応する値とした) 辞書を受け取ります。余分なキーワード引数がない場合には、空の (新たな) 辞書を受け取ります。


なんと、`class`も使える。

```
=> (defclass FooBar [object]
...   "Yet Another Example Class"
...   [[--init--
...     (fn [self x]
...       (setv self.x x)
...       ; Currently needed for --init-- because __init__ needs None
...       ; Hopefully this will go away :)
...       None)]
...
...    [get-x
...     (fn [self]
...       "Return our copy of x"
...       self.x)]])
=> (setv foobar (FooBar '(1 2 3)))
=> (foobar.get-x)
(1L 2L 3L)
```

「class-level attributes」というものもある。
以下の例がチュートリアルにあるが、エラーが出る。オブジェクト指向は分らないのでスルーしてしまいます。

```
(defclass Customer [models.Model]
  [[name (models.CharField :max-length 255})]
   [address (models.TextField)]
   [notes (models.TextField)]])
```

## Hy <-> Python interop

ファイル`greetings.hy`

> (defn greet [name] (print "hello from hy," name))

があったとき、以下のようにpythonからhyを呼び出すことができる?

```
import hy
import greetings

greetings.greet("Foo")
```

逆に、`greetings.py`

> def greet(name):
>    print("hello, %s" % (name))

があった時、hyからは、以下のように使える。

```
(import greetings)
(.greet greetings "foo")
```

キーワード引数を使う例を`greetings.py`があったときで見ると、

>def greet(name, title="Sir"):
>    print("Greetings, %s %s" % (title,name))

```
(import greetings)
(.greet greetings "Foo")
(.greet greetings "Foo" "Darth")
(apply (. greetings greet) ["Foo"] {"title" "Lord"})
```

出力は以下のようになるはず。

```
Greetings, Sir Foo

Greetings, Darth Foo

Greetings, Lord Foo
```

## Protips!

Clujureのスレッディングマクロ`->`が使えます。

`loop (print (eval (read))))` が`(-> (read) (eval) (print) (loop))`となるやつです。

pythonのshライブラリを利用すると、

```
$ pip install sh
$ hy
=> (import [sh [cat grep wc]])
=> (-> (cat "../README.md") (grep "-E" "^h") (wc "-l"))
1
```

とりあえず、以上でチュートリアルは終わりです。

## 感想

かなりpythonチックなLispでした。
Clojureに慣れていたため、かなり違和感を感じましたが、これは慣れの問題だと思います。
pythonを利用してガシガシ何かを作るためのグルー言語みたいでした。
