---
layout: post
title: Analog時計09
date:   2021-09-06
categories: ClojureScript ブログ
---

# Reagentを外してみたら、案外すんなりできた。

ソースは以下。

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

;; キャンバスの描画コンテキストを取得
(def context (reagent/atom (.getContext @cvs "2d")))

;; キャンバスのstyle要素設定関数
(defn- set-cvs-style [cvs]
  (let [style (.-style @cvs)
        m (js/parseInt mini-side)
        h (js/parseInt (.-clientHeight @view-elm))
        w (js/parseInt (.-clientWidth @view-elm))]
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
;; 描画の原点をキャンバスの中心にセット
(.translate (.getContext @cvs "2d") hankei hankei)


(defn div-with-canvas []
  (let [m (js/parseInt mini-side)
        h (js/parseInt (.-clientHeight @view-elm))
        w (js/parseInt (.-clientWidth @view-elm))]
     [:canvas {:width m, :height m
               :style {:width (str m "px")
                       :height (str m "px")
                       :top (str (/ (- h m) 2) "px")
                       :left (str (/ (- w m) 2) "px")
                       :position "absolute"
                       :boxSizing "border-box"
                       :border "0"
                       :padding "0 0 0 0"
                       :margin "0 0 0 0"}}]))
;;;;;;;;;;;
;; 真ん中の円
(defn mini-centerCircle [cvs r color]
  (let [h (js/parseInt r)
        ctx (.getContext @cvs "2d")]
    (set! (.-lineWidth ctx) 0)
    (.beginPath ctx)
    (aset ctx "fillStyle" color)
    (.arc ctx 0 0 (- h 4) 0 (* js/Math.PI 2))
    (.fill ctx)
    (.stroke ctx)))

(defn centerCircle [cvs]
  (let [h 10
        w  2]
    (do (mini-centerCircle cvs h "black")
        (mini-centerCircle cvs (- h w) "silver"))))

;;;;;;;;;;;
;; 文字盤を描く

;; 外側の円
(def outer-circle-width 3)
(defn outer-circle [cvs]
  (let [ctx (.getContext @cvs "2d")]
    (.beginPath ctx)
    (set! (.-strokeStyle ctx) "#000")
    (set! (.-fillStyle ctx) "#000")
    (set! (.-lineWidth ctx) outer-circle-width)
    (.arc ctx 0 0 (- hankei (/ outer-circle-width 2)) 0 (* 2 js/Math.PI) true)
    (.stroke ctx)))

;; 目盛り
(defn scale [cvs h width height]
  (let [ctx (.getContext @cvs "2d")]
    (.beginPath ctx)
    (set! (.-lineWidth ctx) width)
    (.moveTo ctx 0 h)
    (.lineTo ctx 0 (- h height))
    (.stroke ctx)))

(defn scale-string [cvs]
  (let [rot (/ js/Math.PI 6)
        position (- hankei outer-circle-width 30)
        ctx (.getContext @cvs "2d")]
    ;; 文字の基準位置・フォントの設定
    (set! (.-textAlign ctx) "center")
    (set! (.-textBaseline ctx) "middle")
    (set! (.-font ctx) "bold 1.5em sans-serif")
    (doseq [i (range 0 12)]
      (let [deg (* i rot)
            x (* position (js/Math.sin deg))
            y (* (- position) (js/Math.cos deg))]
        (cond (= i 0) (.fillText ctx "12" x y)
              :else (.fillText ctx i x y))))))

(defn all-scale [cvs]
  (outer-circle cvs)
  (.save (.getContext @cvs "2d"))
  (doseq [x (range 0 60)]
    (cond (= 0 (mod x 5)) (scale cvs (- hankei 3) 5 10)
          :else (scale cvs (- hankei 3) 3 5))
    (.rotate (.getContext @cvs "2d") (* (/ js/Math.PI 180) 6)))
  (.restore (.getContext @cvs "2d"))
  (scale-string cvs))

(defn mount []
  (rdom/render [div-with-canvas] (gdom/getElement "app")))

;(js/setInterval mount 1000)

;(defn ^:after-load on-reload []
;  (mount))

(defn clock-main []
  ;; キャンバスをHTMLに追加
  (.appendChild @view-elm @cvs)
  ;; 中央の円を描画
  (centerCircle cvs)
  ;; 目盛りを描画
  (all-scale cvs))
```

Reagentを外すことで、中央の円も目盛りもすんなりできました。

デジタル時計のサンプルを見直すのはしばらく先になりそうです。

