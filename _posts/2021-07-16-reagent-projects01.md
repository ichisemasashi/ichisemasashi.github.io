---
layout: post
title: Reagentのサンプル写経開始
date:   2021-07-16
categories: ClojureScript ブログ
---

# まずはデフォルトのWebページを生成する

~/.clojure/deps.ednにおけるclj-newの設定を利用して、プロジェクトを生成する。

```
% clj -X:new :template figwheel-main :name my-reagent-project/sample :args '["+deps" "--reagent"]'
Generating fresh figwheel-main project.
  To get started:
  -->  Change into the 'sample' directory
  -->  Start build with 'clojure -A:fig:build'


% cd sample 
% tree .
.
├── README.md
├── deps.edn
├── dev.cljs.edn
├── figwheel-main.edn
├── resources
│   └── public
│       ├── css
│       │   └── style.css
│       ├── index.html
│       └── test.html
├── src
│   └── my_reagent_project
│       └── sample.cljs
├── target
│   └── public
├── test
│   └── my_reagent_project
│       ├── sample_test.cljs
│       └── test_runner.cljs
└── test.cljs.edn

9 directories, 11 files
```

この状態で、Learn ClojureScriptでは、`clj -X:new hogehoge`だったが生成されたREADME.mdを見ると`clj`でなく`clojre`だったのでREADME.md形式で実行すると、デフォルトのwebページが表示された。


```
% clojure -A:fig:build
DEPRECATED: Libs must be qualified, change reagent => reagent/reagent (deps.edn)
WARNING: Use of :main-opts with -A is deprecated. Use -M instead.
2021-07-16 23:05:03.525:INFO::main: Logging initialized @4159ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Validating figwheel-main.edn
[Figwheel] figwheel-main.edn is valid \(ツ)/
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 9.589 seconds.
[Figwheel] Outputting main file: target/public/cljs-out/dev-main-auto-testing.js
[Figwheel] Watching paths: ("test" "src") to compile build - dev
[Figwheel] Starting Server at http://localhost:9500
[Figwheel] Starting REPL
Prompt will show when REPL connects to evaluation environment (i.e. a REPL hosting webpage)
Figwheel Main Controls:
          (figwheel.main/stop-builds id ...)  ;; stops Figwheel autobuilder for ids
          (figwheel.main/start-builds id ...) ;; starts autobuilder focused on ids
          (figwheel.main/reset)               ;; stops, cleans, reloads config, and starts autobuilder
          (figwheel.main/build-once id ...)   ;; builds source one time
          (figwheel.main/clean id ...)        ;; deletes compiled cljs target files
          (figwheel.main/status)              ;; displays current state of system
Figwheel REPL Controls:
          (figwheel.repl/conns)               ;; displays the current connections
          (figwheel.repl/focus session-name)  ;; choose which session name to focus on
In the cljs.user ns, controls can be called without ns ie. (conns) instead of (figwheel.repl/conns)
    Docs: (doc function-name-here)
    Exit: :cljs/quit
 Results: Stored in vars *1, *2, *3, *e holds last exception object
[Rebel readline] Type :repl/help for online help info
Opening URL http://localhost:9500
ClojureScript 1.10.773
cljs.user=> 
```

ひとつめのサンプル用にソースを書き換える。

src/my_reagent_project/sample.cljs 

```Clojure
(ns ^:figwheel-hooks my-reagent-project.sample
  (:require
   [reagent.core :as reagent :refer [atom]]
   [reagent.dom :as rdom]))

(println "This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.")

(defn multiply [a b] (* a b)) ;; エラーが出るので残している。

(defn simple-component []
  [:div
   [:p "I am a component!"]
   [:p.someclass
    "I have " [:strong "bold"]
    [:span {:style {:color "red"}} " and red "]
    "text."]])

(defn mount []
  (rdom/render [simple-component] (.-body js/document)))

(mount)

(defn ^:after-load on-reload []
  (mount))
```

![WebPage](../../../../../_posts/img/2021-07-16-webpage.png)

