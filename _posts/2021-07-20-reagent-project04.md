---
layout: post
title: Reagentのサンプル写経その4
date:   2021-07-20
categories: ClojureScript ブログ
---

## 状態管理は、Reagentのatomを利用する

ClojureにもともとあるatomでなくReagentのatomを利用することで、状態が変更されるごとに再レンダリングされるという機能が付加されます。

ついでに、`mount`関数内のrenderの引数をデフォルトに戻してみた。


```Clojure
(ns ^:figwheel-hooks my-reagent-project.sample
  (:require
   [goog.dom :as gdom]
   [reagent.core :as r :refer [atom]]
   [reagent.dom :as rdom]))

(println "This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.")

(defn multiply [a b] (* a b))

(def click-count (r/atom 0))

(defn counting-component []
  [:div
   "The atom " [:code "click-count"] " has value: "
   @click-count ". "
   [:input {:type "button" :value "Click me!"
            :on-click #(swap! click-count inc)}]])

(defn mount []
  (rdom/render [counting-component] (gdom/getElement "app")))

(mount)

(defn ^:after-load on-reload []
  (mount))
```


