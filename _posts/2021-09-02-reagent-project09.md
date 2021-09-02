---
layout: post
title: Reagentのサンプル写経その9
date:   2021-09-02
categories: ClojureScript ブログ
---


# デジタル時計の色を指定するデモ

まず、Figwheel-mainを立ち上げる。

```
% clojure -A:fig:build                         
2021-09-02 20:42:34.232:INFO::main: Logging initialized @4703ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Validating figwheel-main.edn
[Figwheel] figwheel-main.edn is valid \(ツ)/
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 2.215 seconds.
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

ソースコードは、

```
(ns ^:figwheel-hooks my-reagent-project.sample
  (:require
   [goog.dom :as gdom]
   [reagent.core :as r :refer [atom]]
   [reagent.dom :as rdom]
   [clojure.string :as str]))

(println "This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.")

(defn multiply [a b] (* a b))

(defonce timer (r/atom (js/Date.)))

(defonce time-color (r/atom "#f34"))

(defonce time-updater (js/setInterval
                       #(reset! timer (js/Date.)) 1000))

(defn greeting [message]
  [:h1 message])

(defn clock []
  (let [time-str (-> @timer .toTimeString (str/split " ") first)]
    [:div.example-clock
     {:style {:color @time-color}}
     time-str]))

(defn color-input []
  [:div.color-input
   "Time color: "
   [:input {:type "text"
            :value @time-color
            :on-change #(reset! time-color (-> % .-target .-value))}]])

(defn simple-example []
  [:div
   [greeting "Hello world, it is now"]
   [clock]
   [color-input]])


(defn mount []
  (rdom/render [simple-example] (gdom/getElement "app")))

(mount)

(defn ^:after-load on-reload []
  (mount))
```


