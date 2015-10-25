■ http://clojure.org/runtime_polymorphism

Runtime Polymorphism	→	実行時Polymorphism
Systems that utilize runtime polymorphism are easier to change and extend.	→	実行時ポリエステル繊維モルフィズムを利用するシステムは、変えて、広げるのがより簡単です。
Clojure supports polymorphism in several ways:	→	Clojureは、いくつかの点でポリエステル繊維モルフィズムを支持します：
Most core infrastructure data structures in the Clojure runtime are defined by Java interfaces.	→	Clojure実行時の大部分の中心的な基盤データ構造は、Javaインターフェースによって定義されます。
Clojure supports the generation of implementations of Java interfaces in Clojure using proxy (see JVM Hosted).	→	代理（JVM Hostedを見ます）を用いたClojureで、ClojureはJavaインターフェースの機能の生成を支持します。
The Clojure language supports polymorphism along both class and custom hierarchies with multimethods.	→	Clojure言語は、マルチ方法でクラスとカスタムメイドの階層に沿ってポリエステル繊維モルフィズムをサポートします。
The Clojure language also supports a faster form of polymorphism with protocols (but limited only to class polymorphism to take advantage of the JVMs existing capabilities for invocation).	→	プロトコル（しかし、JVMの有利さに祈りの既存の資格を持っていくためにクラス・ポリエステル繊維モルフィズムだけに限られる）で、Clojure言語も、ポリエステル繊維モルフィズムのより速い形をサポートします。
Clojure multimethods are a simple yet powerful mechanism for runtime polymorphism that is free of the trappings of OO, types and inheritance.	→	Clojureマルチ方法は、OO、タイプと遺産の装飾がない実行時ポリエステル繊維モルフィズムのための単純であるが、強力なメカニズムです。
The basic idea behind runtime polymorphism is that a single function designator dispatches to multiple independently-defined function definitions based upon some value of the call.	→	実行時ポリエステル繊維モルフィズムの後の基本的な考えは、独りの機能指名者が独立してチェーン店に急ぐということです定義された呼び出しの若干の価値に基づく機能定義。
For traditional single-dispatch OO languages that value is the type of the 'receiver' or 'this'.	→	伝統的な一回の急送OO言語については、その価値は、『レシーバー』または『これ』のタイプです。
CLOS generic functions extend dispatch value to a composite of the type or value of multiple arguments, and are thus multimethods.	→	一般的な機能が広げるCLOSは、複数の議論のタイプまたは価値の複合物に価格を送って、このようにマルチ方法です。
Clojure multimethods go further still to allow the dispatch value to be the result of an arbitrary function of the arguments.	→	Clojureマルチ方法は、急送価値を議論の任意の機能の結果でいさせるために、さらにまだ行きます。
Clojure does not support implementation inheritance.	→	Clojureは、実施遺産を支えません。
Multimethods are defined using defmulti, which takes the name of the multimethod and the dispatch function.	→	Multimethodsはdefmultiを使って定められます。そして、それはマルチ方法と急送機能の名前をとります。
Methods are independently defined using defmethod, passing the multimethod name, the dispatch value and the function body	→	方法はdefmethodを使って独立して定められます。そして、マルチ方法名、急送価値と機能体を渡します。

(defmulti encounter (fn [x y] [(:Species x) (:Species y)]))
(defmethod encounter [:Bunny :Lion] [b l] :run-away)
(defmethod encounter [:Lion :Bunny] [l b] :eat)
(defmethod encounter [:Lion :Lion] [l1 l2] :fight)
(defmethod encounter [:Bunny :Bunny] [b1 b2] :mate)
(def b1 {:Species :Bunny :other :stuff})
(def b2 {:Species :Bunny :other :stuff})
(def l1 {:Species :Lion :other :stuff})
(def l2 {:Species :Lion :other :stuff})
(encounter b1 b2)
-> :mate
(encounter b1 l1)
-> :run-away
(encounter l1 b1)
-> :eat
(encounter l1 l2)
-> :fight



Multimethods are in every respect fns, e.g. can be passed to map etc.	→	Multimethodsはあらゆる点でfnsで、例えば地図その他に通過されることができます。
Similar to interfaces, Clojure protocols define only function specifications (no implementation) and allow types to implement multiple protocols.	→	インターフェースと同様で、Clojureプロトコルは機能仕様（実施でない）だけを定めて、タイプを複数のプロトコルを実行させます。
Additionally, protocols are open to later dynamic extension for new types.	→	その上、プロトコルは新しいタイプのために後のダイナミックな拡張を受け入れます。
Protocols are limited just to dispatch on class type to take advantage of the native Java performance of polymorphic method calls.	→	多形方法呼び出しの生まれつきのJava実行を利用するために、プロトコルはクラス・タイプ上でちょうど急送に限られています。
For more details, see the protocols page	→	詳細は、プロトコル・ページを参照してください。

