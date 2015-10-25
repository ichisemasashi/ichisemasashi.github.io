■ http://clojure.org/jvm_hosted

Hosted on the JVM	→	JVMで主催されます
Clojure is designed to be a hosted language, sharing the JVM type system, GC, threads etc. It compiles all functions to JVM bytecode.	→	主催された言語、JVMタイプ・システムを共有すること、GC、糸その他であるように、Clojureは設計されています。そして、ItはJVMバイトコードにすべての機能をコンパイルします。
Clojure is a great Java library consumer, offering the dot-target-member notation for calls to Java.	→	Clojureは偉大なJava図書館消費者です。そして、Javaに点-目標会員表記法を呼び出しに対して提供します。
Class names can be referenced in full, or as non-qualified names after being imported.	→	クラス名は、インポートされた後に、完全であるか、資格がない名前でリファレンスをつけられることができます。
Clojure supports the dynamic implementation of Java interfaces and classes using reify and proxy:	→	Clojureは、Javaインターフェースのダイナミックな実施とreifyと代理を用いたクラスをサポートします：
Here's a small Swing app:	→	小さなSwingアプリは、ここにあります：

(import '(javax.swing JFrame JLabel JTextField JButton)
        '(java.awt.event ActionListener)
        '(java.awt GridLayout))
(defn celsius []
  (let [frame (JFrame. "Celsius Converter")
        temp-text (JTextField.)
        celsius-label (JLabel. "Celsius")
        convert-button (JButton. "Convert")
        fahrenheit-label (JLabel. "Fahrenheit")]
    (.addActionListener
     convert-button
     (reify ActionListener
            (actionPerformed
             [_ evt]
             (let [c (Double/parseDouble (.getText temp-text))]
               (.setText fahrenheit-label
                         (str (+ 32 (* 1.8 c)) " Fahrenheit"))))))
    (doto frame
      (.setLayout (GridLayout. 2 2 3 3))
      (.add temp-text)
      (.add celsius-label)
      (.add convert-button)
      (.add fahrenheit-label)
      (.setSize 300 80)
      (.setVisible true))))
(celsius)


http://clojure.org/file/view/celsius.jpeg/34601359/celsius.jpeg

