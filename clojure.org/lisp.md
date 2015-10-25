■ http://clojure.org/lisp

Clojure as a dialect of Lisp	→	LISPの方言としてのClojure
Clojure is a member of the Lisp family of languages.	→	Clojureは、言語のLISP家族のメンバーです。
Many of the features of Lisp have made it into other languages, but Lisp's approach to code-as-data and its macro system still set it apart.	→	LISPの特徴の多くはそれを他の言語にしました、しかし、データとしてのコードへのLISPのアプローチとそのマクロ・システムはまだそれを引き離します。
Clojure extends the code-as-data system beyond parenthesized lists (s-expressions) to vectors and maps.	→	Clojureは、ベクトルと地図に括弧に入れられたリスト（s-表現）を越えて、データとしてのコード・システムを広げます。
Thus vectors and maps can be used in macro syntax, have literal reader representations etc.	→	このように、ベクトルと地図が、マクロ構文で使われることができて、文字通りの読者表現その他を持ちます。
Lisp data, and thus Lisp code, is read by the reader.	→	データを舌足らずに言ってください、そして、このように、LISPコードは読者によって読まれます。
The result of reading is the data structure represented by the forms.	→	読書の結果は、形によって意味されるデータ構造です。
Clojure can compile data structures that represent code, and as part of that process it looks for calls to macros.	→	Clojureはコードを意味するデータ構造をコンパイルすることができます、そして、そのプロセスの一部として、それはマクロに呼び出しを探します。
When it sees one, it calls the macro, passing the forms themselves as arguments, then uses the return value of the macro in place of the macro itself.	→	1を見るとき、それから、マクロが、議論として形に自分自身を渡して、マクロそのものの代わりにマクロの戻り値を使うと、それは叫びます。
Thus macros are functions that are called at compile time to perform transformations of code.	→	このように、マクロは、コードの変化を実行するためにコンパイル時で呼ばれる機能です。
Since code is data, all of the Clojure library is available to assist in the transformation.	→	コードがデータであるので、Clojure図書館の全ては変化面で助けることが可能です。
Thus macros allow Lisps, and Clojure, to support syntactic abstraction.	→	このように、統語的な抽象化を支持するために、マクロはLispsとClojureを許します。
You use macros for the same reasons you use functions - to eliminate repetition in your code.	→	あなたは機能を使用する同じ理由のためにマクロを使用します－あなたのコードで繰り返しを除きます。
Macros should be reserved for situations for which functions are insufficient, e.g. when you need to control evaluation, generate identifiers etc. Many of the core constructs of Clojure are not built-in primitives but macros just like users can define.	→	例えばあなたが評価をコントロールする必要があるとき、マクロは機能が不十分である状況のために予約でなければなりません識別子その他を生み出します、ユーザーが定義することができるように、Clojureの中心的な構成概念のManyはビルトイン・プリミティブでなくマクロです。
Here's and:	→	乾杯、そして：

(defmacro and
  ([] true)
  ([x] x)
  ([x & rest]
    `(let [and# ~x]
       (if and# (and ~@rest) and#))))

構文引用句（）（形が彼らが生み出す形を模倣するマクロを定めることを簡単にします）の用法に注意してください。

