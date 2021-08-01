---
layout: post
title: Reagentのサンプル写経その7
date:   2021-08-01
categories: ClojureScript ブログ
---

## 必須API

Reagentのほとんどのアプリケーションで必要となるエントリーポイントは本当に1つだけです：`reagent.dom/render`です。

これは2つの引数を取ります。コンポーネントとDOMノードです。

```
% clojure -A:fig:build                        
DEPRECATED: Libs must be qualified, change reagent => reagent/reagent (deps.edn)
WARNING: Use of :main-opts with -A is deprecated. Use -M instead.
2021-08-01 20:59:11.489:INFO::main: Logging initialized @4712ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Validating figwheel-main.edn
[Figwheel] figwheel-main.edn is valid \(ツ)/
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 3.277 seconds.
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
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 1.385 seconds.
[Figwheel] Outputting main file: target/public/cljs-out/dev-main-auto-testing.js
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 0.795 seconds.
[Figwheel] Outputting main file: target/public/cljs-out/dev-main-auto-testing.js
This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.
cljs.user=> 
```


ソースを編集して

```Clojure
(ns ^:figwheel-hooks my-reagent-project.sample
  (:require
   [goog.dom :as gdom]
   [reagent.core :as r :refer [atom]]
   [reagent.dom :as rdom]))

(println "This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.")

(defn multiply [a b] (* a b))

(defn simple-component []
  [:div
   [:p "I am a component!"]
   [:p.someclass
    "I have " [:strong "bold"]
    [:span {:style {:color "red"}} "and red "] "text."]])

(defn mount []
  (rdom/render [simple-component] (gdom/getElement "app")))

(mount)

(defn ^:after-load on-reload []
  (mount))
```

