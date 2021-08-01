---
layout: post
title: Reagentのサンプル写経その6
date:   2021-07-27
categories: ClojureScript ブログ
---

## コンポーネントは要するに関数なので、`reagent/atom`を渡すことでコンポーネント間で状態を共有できる

ソースを書き換える。

```Clojure
(ns ^:figwheel-hooks my-reagent-project.sample
  (:require
   [goog.dom :as gdom]
   [reagent.core :as r :refer [atom]]
   [reagent.dom :as rdom]))

(println "This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.")

(defn multiply [a b] (* a b))

(defn atom-input [val]
  [:input {:type "text"
           :value @val
           :on-change #(reset! val (-> % .-target .-value))}])

(defn shared-state []
  (let [val (r/atom "foo")]
    (fn []
      [:div
       [:p "The value is now: " @val]
       [:p "Change it here: " [atom-input val]]])))
                              ;; ↑ コンポーネント間で`atom`を共有している。

(defn mount []
  (rdom/render [shared-state] (gdom/getElement "app")))

(mount)

(defn ^:after-load on-reload []
  (mount))

```

