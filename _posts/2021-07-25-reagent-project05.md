---
layout: post
title: Reagentのサンプル写経その5
date:   2021-07-25
categories: ClojureScript ブログ
---

## コンポーネントがレンダリングされるたびに`setTimeout`を呼びだしてカウンターを更新

まずは、Figwheel-mainを起動する。

```
% clojure -A:fig:build                              
2021-07-25 15:59:23.535:INFO::main: Logging initialized @4432ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Validating figwheel-main.edn
[Figwheel] figwheel-main.edn is valid \(ツ)/
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 1.878 seconds.
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
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 1.038 seconds.
[Figwheel] Outputting main file: target/public/cljs-out/dev-main-auto-testing.js
This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.
cljs.user=> 
```

ソースファイルの編集は以下のようにする。

```Clojure
(ns ^:figwheel-hooks my-reagent-project.sample
  (:require
   [goog.dom :as gdom]
   [reagent.core :as r :refer [atom]]
   [reagent.dom :as rdom]))

(println "This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.")

(defn multiply [a b] (* a b))

(defn timer-component []
  (let [seconds-elapsed (r/atom 0)]
    (fn []
      (js/setTimeout #(swap! seconds-elapsed inc) 1000)
      [:div "Seconds Elaspsed: " @seconds-elapsed])))

(defn mount []
  (rdom/render [timer-component] (gdom/getElement "app")))

(mount)

(defn ^:after-load on-reload []
  (mount))
```

コンポーネント関数は、実際のレンダリングをする別の関数を返している。

この関数は最初の関数と同じ引数で呼び出される。

*これにより、Reactのライフサイクルイベントに頼ることなく、新しく作成されたコンポーネントのセットアップを行うことができます。*
