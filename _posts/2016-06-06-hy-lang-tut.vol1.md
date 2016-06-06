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

今日のログ

```
ichisemasashi:~/workspace $ script 2016-06-06-hy-tutu.vol.1.txt
Script started, file is 2016-06-06-hy-tutu.vol.1.txt
ichisemasashi:~/workspace $ cd Hy
ichisemasashi:~/workspace/Hy $ source bin/activate
(Hy)ichisemasashi:~/workspace/Hy $ ifconfig -a | grep inet
          inet addr:172.17.23.68  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe11:1744/64 Scope:Link
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
(Hy)ichisemasashi:~/workspace/Hy $ hy
hy 0.11.1 using CPython(default) 2.7.6 on Linux
=> (print "hello world")
hello world
=> (+ 1 3)
4L
=> ; basic hello world
=> (+ 1 3 55)
59L
=> (setv result (- (/ (+ 1 3 88) 2) 8))
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
=> [1 2 3]
[1L, 2L, 3L]
=> {"dog" "bark"
... "cat" "meow"}
{u'dog': u'bark', u'cat': u'meow'}
=> (, 1 2 3)
(1L, 2L, 3L)
=> '(1 2 3)
(1L 2L 3L)
=> (.strip " fooooo   ")
u'fooooo'
=> (setv this-string " fooooo   ")
=> (this-string.strip)
u'fooooo'
=> (if (try-some-thing)
...   (print "this is if true")
...   (print "this is if false"))
Traceback (most recent call last):
  File "<input>", line 1, in <module>
NameError: name 'try_some_thing' is not defined
=> (if true
...   (print "this is if true")
...   (print "this is if false")
... )
this is if true
=> (setv somevar 1000)
=> (cond
...  [(> somevar 50)
...   (print "That variable is too big!")]
...  [(< somevar 10)
...   (print "That variable is too small!")]
...  [true
...   (print "That variable is jussssst right!")])
That variable is too big!
=> (setv try-some-thing false)
=> (if (try-some-thing)
...   (do
...     (print "this is if true")
...     (print "and why not, let's keep talking about how true it is!))
...   (print "this one's still simply just false"))
... )
... )
... )
... )
...
KeyboardInterrupt
=> try-some-thing
False
=> (if (try-some-thing)
...   (do
...     (print "this is if true")
...     (print "and why not, let's keep talking about how true it is!"))
...   (print "this one's still simply just false"))
Traceback (most recent call last):
  File "<input>", line 1, in <module>
TypeError: 'bool' object is not callable
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
=> (print "this will run")
this will run
=> ; (print "but this will not")
=> (+ 1 2 3)  ; we'll execute the addition, but not this comment!
6L
=>
(Hy)ichisemasashi:~/workspace/Hy $ deactivate
ichisemasashi:~/workspace/Hy $ date
Mon Jun  6 10:52:53 UTC 2016
ichisemasashi:~/workspace/Hy $ exit
exit
Script done, file is 2016-06-06-hy-tutu.vol.1.txt
ichisemasashi:~/workspace $
```
