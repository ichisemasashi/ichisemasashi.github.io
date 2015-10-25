■ http://clojure.org/dynamic

Dynamic Development	→	ダイナミックな発展
First and foremost, Clojure is dynamic.	→	まっさきに、Clojureはダイナミックです。
That means that a Clojure program is not just something you compile and run, but something with which you can interact.	→	つまり、Clojureプログラムは、ちょうどあなたが編集して、走らせる何かでなく、あなたが交流することができる何かです。
Clojure is not a language abstraction, but an environment, where almost all of the language constructs are reified, and thus can be examined and changed.	→	Clojureは言語抽象概念でなく、環境です、そこで、言語構成概念のほぼ全てはreifiedされて、このように調べられることができて、変わることができます。
This leads to a substantially different experience from running a program, examining its results (or failures) and trying again.	→	プログラムを実行して、その結果（または失敗）を調べて、再びためすこととかなり異なる経験に、これは至ります。
In particular, you can grow your program, with data loaded, adding features, fixing bugs, testing, in an unbroken stream.	→	特に、データをロードして、あなたはプログラムを発達させることができます。そして、特徴を加えます。そして、完全な流れでバグ（テスト）を修正します。
The REPL	→	REPL
While Clojure can be embedded in a Java application, or used as a scripting language, the primary programming interface is the Read-Eval-Print-Loop (REPL).	→	ClojureがJavaアプリケーションに埋め込まれることができるか、スクリプト言語として使われることができる間、主要なプログラミング・インターフェースはRead-Eval-Print-Loop（REPL）です。
This is a simple console interface that allows you to enter and execute commands, and examine their results.	→	これは、司令部に入って、実行して、彼らの結果を調べることができる単純なコンソール・インターフェースです。
You can start the Clojure REPL like this, and then follow along trying the samples in this feature tour:	→	あなたはこのようにClojure REPLを始めることができて、そして、この特徴ツアーでサンプルをためすことに沿ってあとに続きます：

$ java -cp clojure.jar clojure.main

これは、このようにあなたに注意をします：

user=>

大部分のClojure命令は、形（命令arguments*）をとります。
それをためしてください：

(def x 6)
-> #'user/x
(def y 36)
-> #'user/y
(+ x y)
-> 42

基本
Clojureには、任意の精度整数、ひも、比率、ダブルス、性格、シンボル、キーワードがあります。

(* 12345678 12345678)
-> 152415765279684
"string"
-> "string"
22/7
-> 22/7
3.14159
-> 3.14159
\a
-> \a
'symbol
-> symbol
:keyword
-> :keyword
;a comment


Dynamic Compilation	→	ダイナミックな編集
Clojure is a compiled language, so one might wonder when you have to run the compiler.	→	Clojureは編集された言語であるので、人はあなたがいつコンパイラを実行しなければならないかについて疑問に思うかもしれません。
You don't.	→	あなたは、そうしません。
Anything you enter into the REPL or load using load-file is automatically compiled to JVM bytecode on the fly.	→	REPLまたはロード・ファイルを使用しているロードに、あなたが入れる何でも、自動的にその場でJVMバイトコードに自動的に編集されます。
Compiling ahead-of-time is also possible, but not required	→	事前に編集することができもするが、要求しませんでした。


