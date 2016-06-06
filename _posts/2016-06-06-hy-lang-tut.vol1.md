---
layout: post
title: Hy言語チュートリアルを触ってみる(1)
date:   2016-06-06 10:52:53 +0900
categories: hy-lang
---
# [Hy言語チュートリアル](http://docs.hylang.org/en/stable/tutorial.html)を触ってみる(1)

## Basic intro to Lisp for Pythonistas
かんたんな「hello world」

```
=> (print "hello world")
hello world
```

かんたんな計算

```
=> (+ 1 3)
4L
```

ちょっと引数が増える

```
=> (+ 1 3 55)
59L
```

括弧が増えても心配なし(※ `setv`は`result`に38を代入します。)

```
=> (setv result (- (/ (+ 1 3 88) 2) 8))
=>
```

かんたんなプログラム(※ `defn`はhyのメソッドで、メソッドの芸妓定義するメソッドです。)

```
=> (defn simple-conversation []
...    (print "Hello!  I'd like to get to know you.  Tell me about yourself!")
...    (setv name (raw-input "What is your name? "))
...    (setv age (raw-input "What is your age? "))
...    (print (+ "Hello " name "!  I see you are "
...               age " years old.")))
=>
=> (simple-conversation)
Hello!  I'd like to get to know you.  Tell me about yourself!
What is your name? ichise
What is your age? 44
Hello ichise!  I see you are 44 years old.
```

## Hy is a Lisp-flavored Python
pythonのデータタイプと標準ライブラリへのフルアクセスができます。
リスト、タプル、文字列はデータを一列に並べたデータ構造
辞書は「連想配列」のことで、Perl ではハッシュと呼ばれています。

```
=> [1 2 3]
[1L, 2L, 3L]
=> {"dog" "bark"
... "cat" "meow"}
{u'dog': u'bark', u'cat': u'meow'}
=> (, 1 2 3)
(1L, 2L, 3L)
```

他のLisp同様、クオートが使える。

```
=> '(1 2 3)
(1L 2L 3L)
```

pythonの関数も使える。

    string.strip(s[, chars])
    文字列の先頭と末尾から文字を取り除いたコピーを生成して返します。 chars を指定しない場合や None にした場合、先頭と末尾の空白を取り除きます。 chars を None 以外に指定する場合、 chars は文字列でなければなりません。


```
=> (.strip " fooooo   ")
u'fooooo'
```

文字列を変数に代入してあれば、こんなこともできる。

```
=> (setv this-string " fooooo   ")
=> (this-string.strip)
u'fooooo'
```

条件分岐

```
=> (if true
...   (print "this is if true")
...   (print "this is if false"))
this is if true
```

もっと複雑な条件分岐は`cond`を使う。

```
=> (setv somevar 1000)
=> (cond
...  [(> somevar 50)
...   (print "That variable is too big!")]
...  [(< somevar 10)
...   (print "That variable is too small!")]
...  [true
...   (print "That variable is jussssst right!")])
That variable is too big!
```

`if`の分岐は1つの式しか入らないが、複数をまとめる`do`を使える。

```
=> (defn try-some-thing [] (= 0 1))
=> (if (try-some-thing)
...   (do
...     (print "this is if true")
...     (print "and why not, let's keep talking about how true it is!"))
...   (print "this one's still simply just false"))
this one's still simply just false
=> (defn try-some-thing [] (= 0 0))
=> (if (try-some-thing)
...   (do
...     (print "this is if true")
...     (print "and why not, let's keep talking about how true it is!"))
...   (print "this one's still simply just false"))
this is if true
and why not, let's keep talking about how true it is!
```

コメントは、「;」(セミコロン)

```
=> (print "this will run")
this will run
=> ; (print "but this will not")
=> (+ 1 2 3)  ; we'll execute the addition, but not this comment!
6L
```
