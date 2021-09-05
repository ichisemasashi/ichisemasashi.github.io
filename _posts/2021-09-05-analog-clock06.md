---
layout: post
title: Analog時計06
date:   2021-09-05
categories: ClojureScript ブログ
---

# Reagentのレンダーを導入&ちょっとしたバグ修正

キャンバス要素が中央に寄ってなかった点を修正。

```Clojure
(ns ^:figwheel-hooks my-cljs-projects.analog-clock
  (:require
   [goog.dom :as gdom]
   [reagent.core :as reagent :refer [atom]]
   [reagent.dom :as rdom]))

(println "This text is printed from src/my_cljs_projects/analog_clock.cljs. Go ahead and edit it and see reloading in action.")

;; div要素を取得
(def view-elm (reagent/atom (gdom/getElement "app")))

;; div要素の縦横の短い方を取得
(def mini-side (min (.-clientWidth @view-elm) (.-clientHeight @view-elm)))
;; 時計の半径を取得
(def hankei (/ mini-side 2))
;; キャンバスの作成
(def cvs (reagent/atom (gdom/createElement "canvas")))

;; キャンバスの描画サイズセット
(.setAttribute @cvs "width" mini-side)
(.setAttribute @cvs "height" mini-side)

;; キャンバスのstyle要素設定関数
(defn- set-cvs-style [cvs]
  (let [style (.-style @cvs)
        m (js/parseInt mini-side)
        h (js/parseInt view-elm.clientHeight)
        w (js/parseInt view-elm.clientWidth)]
    (do ;; キャンバスの表示サイズセット
      (set! (.-width style) (str m "px"))
      (set! (.-height style) (str m "px"))
      ;; キャンバスをdiv要素の中央にセット
      (set! (.-top style)
            (str (/ (- h m) 2) "px"))
      (set! (.-left style)
            (str (/ (- w m) 2) "px"))
      ;; キャンバスにスタイルをセット
      (set! (.-position style) "absolute")
      (set! (.-boxSizing style) "border-box")
      (set! (.-border style) "0")
      (set! (.-padding style) "0 0 0 0")
      (set! (.-margin style) "0 0 0 0"))))

(set-cvs-style cvs)

;; キャンバスの描画コンテキストを取得
(def context (reagent/atom (.getContext @cvs "2d")))

;; 描画の原点をキャンバスの中心にセット
(.translate @context hankei hankei)

(defn div-with-canvas []
  (let [m (js/parseInt mini-side)
        h (js/parseInt (.-clientHeight @view-elm))
        w (js/parseInt (.-clientWidth @view-elm))]
    [:div.with-canvas
     [:canvas {:width mini-side, :height mini-side
               :style {:width (str m "px")
                       :height (str m "px")
                       :top (str (/ (- h m) 2) "px")
                       :left (str (/ (- w m) 2) "px")
                       :position "absolute"
                       :boxSizing "border-box"
                       :border "0"
                       :padding "0 0 0 0"
                       :margin "0 0 0 0"}}]]))

(defn mount []
  (rdom/render [div-with-canvas] (gdom/getElement "app")))

(mount)

(defn ^:after-load on-reload []
  (mount))
```