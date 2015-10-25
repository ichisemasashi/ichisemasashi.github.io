■ http://clojure.org/concurrent_programming

Concurrent Programming	→	コンカレント・プログラミング
Today's systems have to deal with many simultaneous tasks and leverage the power of multi-core CPUs.	→	今日のシステムは多くの同時の仕事に対処しなければならなくて、マルチコアＣＰＵの力に影響を及ぼさなければなりません。
Doing so with threads can be very difficult due to the complexities of synchronization.	→	糸でそうすることは、同期の複雑さのために、非常に難しくありえます。
Clojure simplifies multi-threaded programming in several ways.	→	Clojureは、いくつかの点でマルチスレッドのプログラミングを単純化します。
Because the core data structures are immutable, they can be shared readily between threads.	→	中心的なデータ構造が不変であるので、彼らはすぐに糸で分配されることができます。
However, it is often necessary to have state change in a program.	→	しかし、プログラムにおける状態変化があることが、しばしば必要です。
Clojure, being a practical language, allows state to change but provides mechanism to ensure that, when it does so, it remains consistent, while alleviating developers from having to avoid conflicts manually using locks etc. The software transactional memory system (STM), exposed through dosync, ref, set, alter, et al, supports sharing changing state between threads in a synchronous and coordinated manner.	→	Clojure（実用的な言葉遣いであること）は、州が変わるのを許すが、それを確実にするために、メカニズムを提供します。そのとき、それはそうします、それは首尾一貫したままです、手でロックその他を使っている紛争を避けなければならないことから開発者を軽減している間、ソフトウェア処理の記憶システム（STM）は終わりまでdosync、参照、セットを露出させました、変えます、ほか、同期で調整方法で変わっている州を糸で分配している支持物。
The agent system supports sharing changing state between threads in an asynchronous and independent manner.	→	薬品系は、非同期で独立した方法で変わっている州を糸で分配することをサポートします。
The atoms system supports sharing changing state between threads in a synchronous and independent manner.	→	原子系は、同期で独立した方法で変わっている州を糸で分配することをサポートします。
The dynamic var system, exposed through def, binding, et al, supports isolating changing state within threads.In all cases, Clojure does not replace the Java thread system, rather it works with it.	→	ダイナミックなバール・システムは終わりまでdef、装丁、ほかを露出させました。そして、支持物が筋の中で変わっている州を孤立させました。
Clojure functions are java.util.concurrent.	→	Clojure機能は、java.util.concurrentです。
Callable, therefore they work with the Executor framework etc.	→	呼ぶことができて、したがって、彼らはExecutorフレームワークその他で働きます。
Most of this is covered in more detail in the concurrency screencast.	→	これの最大限は、並列性screencastで更に詳細にカバーされます。
Refs are mutable references to objects.	→	レフリーは、物への変わりやすい言及です。
They can be ref-set or altered to refer to different objects during transactions, which are delimited by dosync blocks.	→	彼らは参照-セットでありえるか、業務の間、異なる物に言及するために変わりました。そして、それはdosyncブロックによって区切られます。
All ref modifications within a transaction happen or none do.Also, reads of refs within a transaction reflect a snapshot of the reference world at a particular point in time, i.e. each transaction is isolated from other transactions.	→	業務の範囲内のすべての参照修正は、起こります、または、何もしません。Also、特定の時（すなわち各々の業務）の参照世界のスナップショットが他の業務から分離されると、業務の範囲内のレフリーの読み物は、思います。
If a conflict occurs between 2 transactions trying to modify the same reference, one of them will be retried.	→	コンフリクトが同じ参照を修正しようとしている2つの業務の間で起こるならば、彼らのうちの1人は再び試みられます。
All of this occurs without explicit locking.	→	これの全ては、露骨にロックすることなく起こります。
In this example a vector of Refs containing integers is created (refs), then a set of threads are set up (pool) to run a number of iterations of incrementing every Ref (tasks).	→	この例では、整数を含んでいるRefsのベクトルは作成されます（レフリー）、そして、あらゆるRef（仕事）を増加させるいくつかの繰り返しを走らせるために、一組の糸は準備されます（プール）。
This creates extreme contention, but yields the correct result.	→	これは最大の争いを引き起こすが、正しい結果を与えます。
No locks!	→	いいえはロックします！

(import '(java.util.concurrent Executors))

(defn test-stm [nitems nthreads niters]
  (let [refs  (map ref (repeat nitems 0))
        pool  (Executors/newFixedThreadPool nthreads)
        tasks (map (fn [t]
                      (fn []
                        (dotimes [n niters]
                          (dosync
                            (doseq [r refs]
                              (alter r + 1 t))))))
                   (range nthreads))]
    (doseq [future (.invokeAll pool tasks)]
      (.get future))
    (.shutdown pool)
    (map deref refs)))

(test-stm 10 10 10000)
-> (550000 550000 550000 550000 550000 550000 550000 550000 550000 550000)


In typical use refs can refer to Clojure collections, which, being persistent and immutable, efficiently support simultaneous speculative 'modifications' by multiple transactions.	→	典型的使用法では、レフリーはClojureコレクションに言及することができます。そして、それは、持続的で不変で、同時思惑的な『修正』を複数の業務で効率的に支えます。
Mutable objects should not be put in refs.	→	変わりやすい物は、レフリーに入れられてはいけません。
By default Vars are static, but per-thread bindings for Vars defined with metadata mark them as dynamic.	→	デフォルトで、Varsは動かないです、しかし、ダイナミックであるように、メタデータで定められるVarsのための糸につき装丁は彼らをマークします。
Dynamic vars are also mutable references to objects.	→	ダイナミックなバールは、物への変わりやすい言及でもあります。
They have a (thread-shared) root binding which can be established by def, and can be set using set!, but only if they have been bound to a new storage location thread-locally using binding.	→	彼らは、defによって確立されることができる、（糸共有された）根装丁を持って、セットを使って配置されることができます！彼らが糸地元で装丁を使っている新しい貯蔵場所に行くところの場合だけ、しかし。
Those bindings and any subsequent modifications to those bindings will only be seen within the thread by code in the dynamic scope of the binding block.	→	それらの装丁とそれらの装丁へのどんな以降の修正でも、拘束的なブロックのダイナミックな範囲で、コードによって筋の中で見られるだけです。
Nested bindings obey a stack protocol and unwind as control exits the binding block	→	支配が拘束的なブロックを出て、入れ子になった包帯はスタック・プロトコルに従って、ほどけます。

(def ^:dynamic *v*)

(defn incv [n] (set! *v* (+ *v* n)))

(defn test-vars [nthreads niters]
  (let [pool (Executors/newFixedThreadPool nthreads)
        tasks (map (fn [t]
                     #(binding [*v* 0]
                        (dotimes [n niters]
                          (incv t))
                        *v*))
                   (range nthreads))]
      (let [ret (.invokeAll pool tasks)]
        (.shutdown pool)
        (map #(.get %) ret))))

(test-vars 10 1000000)
-> (0 1000000 2000000 3000000 4000000 5000000 6000000 7000000 8000000 9000000)
(set! *v* 4)
-> java.lang.IllegalStateException: Can't change/establish root binding of: *v* with set


Dynamic vars provide a way to communicate between different points on the call stack without polluting the argument lists and return values of the intervening calls.In addition, dynamic vars support a flavor of context-oriented programming.	→	ダイナミックなバールは、中の呼び出しの議論リストと戻り値を汚染することなく呼び出しスタックの上で異なる点の間で通信する方法を提供します。
Because fns defined with defn are stored in vars, they too can be dynamically rebound:	→	defnで定められるfnsがバールに保管されるので、彼らもダイナミックに再結合されることができます：

(defn ^:dynamic say [& args]
  (apply str args))

(defn loves [x y]
  (say x " loves " y))

(defn test-rebind []
  (println (loves "ricky" "lucy"))
  (let [say-orig say]
    (binding [say (fn [& args]
                      (println "Logging say")
                      (apply say-orig args))]
      (println (loves "fred" "ethel")))))

(test-rebind)

ricky loves lucy
Logging say
fred loves ethel

