---
layout: post
title: Reagentのサンプル写経その3
date:   2021-07-19
categories: ClojureScript ブログ
---

## シーケンス内のアイテムを表示(:li)

とりあえず、Figwheel-mainの起動までは省略。

ソースを写経して

```Clojure
(ns ^:figwheel-hooks my-reagent-project.sample
  (:require
   [reagent.core :as reagent :refer [atom]]
   [reagent.dom :as rdom]))

(println "This text is printed from src/my_reagent_project/sample.cljs. Go ahead and edit it and see reloading in action.")

(defn multiply [a b] (* a b))

(defn lister [items]
  [:ul
   (for [item items]
     ^{:key item} [:li "Item " item])])

(defn lister-user []
  [:div
   "Here is a list:"
   [lister (range 3)]])

(defn mount []
  (rdom/render [lister-user] (.-body js/document)))

(mount)

(defn ^:after-load on-reload []
  (mount))
```

REPLが起動したあとに、関数を起動してみる。メタデータを見てみたい。しかし、メタデータなので表示されない。

```Clojure
cljs.user=> (in-ns 'my-reagent-project.sample)

my-reagent-project.sample=> (lister-user)
[:div "Here is a list:" [#object[my_reagent_project$sample$lister] (0 1 2)]]
my-reagent-project.sample=> (lister (range 3))
[:ul ([:li "Item " 0] [:li "Item " 1] [:li "Item " 2])]
my-reagent-project.sample=> 
```

【疑問】関数listerが生成するデータの中にメタデータ(`^{:key item}`)がある。これはどのように利用するのだろうか?
Reactのドキュメントを読んでもイマイチだった。


