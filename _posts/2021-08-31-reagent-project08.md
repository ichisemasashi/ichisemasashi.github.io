---
layout: post
title: Reagentのサンプル写経その8
date:   2021-08-31
categories: ClojureScript ブログ
---

すこし時間的間隔が空いたけど、LispTeaTimeでまたCLJS熱が再発しました。


## シンプルなBMI計算機の例

ソースは全部出されているので、貼り付けるだけです。

まずは、コンソールから

```
% clojure -A:fig:build
DEPRECATED: Libs must be qualified, change reagent => reagent/reagent (deps.edn)
WARNING: Use of :main-opts with -A is deprecated. Use -M instead.
2021-08-31 21:17:21.172:INFO::main: Logging initialized @4314ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Validating figwheel-main.edn
[Figwheel] figwheel-main.edn is valid \(ツ)/
[Figwheel:WARNING] Making target directory "target" and re-adding it to the classpath (this only needs to be done when the target directory doesn't exist)
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 9.162 seconds.
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

ソースは

```Clojure
(ns ^:figwheel-hooks my-reagent-project.sample
  (:require
   [goog.dom :as gdom]
   [reagent.core :as r :refer [atom]]
   [reagent.dom :as rdom]))

(println "This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.")

(defn multiply [a b] (* a b))

(defn calc-bmi [{:keys [height weight bmi] :as data}]
  (let [h (/ height 100)]
    (if (nil? bmi)
      (assoc data :bmi (/ weight (* h h)))
      (assoc data :weight (* bmi h h)))))

(def bmi-data
  (r/atom (calc-bmi {:height 180 :weight 80})))

(defn slider [param value min max invalidates]
  [:input {:type "range" :value value :min min :max max
           :style {:width "100%"}
           :on-change (fn [e]
                        (let [new-value (js/parseInt (.. e -target -value))]
                          (swap! bmi-data
                                 (fn [data]
                                   (-> data
                                       (assoc param new-value)
                                       (dissoc invalidates)
                                       calc-bmi)))))}])

(defn bmi-component []
  (let [{:keys [weight height bmi]} @bmi-data
        [color diagnose] (cond (< bmi 18.5) ["orange" "underweight"]
                               (< bmi 25) ["inherit" "normal"]
                               (< bmi 30) ["orange" "overweight"]
                               :else ["red" "obese"])]
    [:div
     [:h3 "BMI calculator"]
     [:div
      "Height: " (int height) "cm"
      [slider :height height 100 220 :bmi]]
     [:div
      "Weight: " (int weight) "kg"
      [slider :weight weight 30 150 :bmi]]
     [:div
      "BMI: " (int bmi) " "
      [:span {:style {color color}} diagnose]
      [slider :bmi bmi 10 50 :weight]]]))

(defn mount []
  (rdom/render [bmi-component] (gdom/getElement "app")))

(mount)

(defn ^:after-load on-reload []
  (mount))
```

