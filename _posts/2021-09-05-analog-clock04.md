---
layout: post
title: Analog時計04
date:   2021-09-05
categories: ClojureScript ブログ
---

# 「キャンバスの作成・初期化」をREPL上で試す

```

;; div要素を取得
my-cljs-projects.analog-clock=> (def view-elm (gdom/getElement "app"))
#'my-cljs-projects.analog-clock/view-elm
my-cljs-projects.analog-clock=> (.-clientWidth view-elm)
1187
my-cljs-projects.analog-clock=> (.-clientHeight view-elm)
400
;; 時計の半径を取得
my-cljs-projects.analog-clock=> (def hankei (/ (min (.-clientWidth view-elm) (.-clientHeight view-elm)) 2))
#'my-cljs-projects.analog-clock/hankei
my-cljs-projects.analog-clock=> hankei
200
;; div要素の縦横の短い方を取得
my-cljs-projects.analog-clock=> (def mini-side (min (.-clientWidth view-elm) (.-clientHeight view-elm)))
#'my-cljs-projects.analog-clock/mini-side
my-cljs-projects.analog-clock=> mini-side
400
;; キャンバスの作成
my-cljs-projects.analog-clock=> (def cvs (gdom/createElement "canvas"))
#'my-cljs-projects.analog-clock/cvs
;; キャンバスの描画サイズセット
my-cljs-projects.analog-clock=> (.setAttribute cvs "width" mini-side)
nil
my-cljs-projects.analog-clock=> cvs
#object[HTMLCanvasElement [object HTMLCanvasElement]]
my-cljs-projects.analog-clock=> (.setAttribute cvs "height" mini-side)
nil
my-cljs-projects.analog-clock=> cvs
#object[HTMLCanvasElement [object HTMLCanvasElement]]
my-cljs-projects.analog-clock=> cvs.width
400
my-cljs-projects.analog-clock=> cvs.height
400
;; キャンバスの表示サイズセット
my-cljs-projects.analog-clock=> (set! cvs.style.width (str mini-side "px"))
"400px"
my-cljs-projects.analog-clock=> cvs.style.width
"400px"
my-cljs-projects.analog-clock=> (set! cvs.style.height (str mini-side "px"))
"400px"
my-cljs-projects.analog-clock=> cvs.style.height
"400px"
;; キャンバスをdiv要素の中央にセット
my-cljs-projects.analog-clock=> (set! cvs.style.top (str (/ (- (js/parseInt view-elm.clientHeight) (js/parseInt mini-side)) 2) "px"))
"0px"
my-cljs-projects.analog-clock=> (set! cvs.style.left (str (/ (- (js/parseInt view-elm.clientWidth) (js/parseInt mini-side)) 2) "px"))
"393.5px"
;; キャンバスにスタイルをセット
my-cljs-projects.analog-clock=> (set! cvs.style.position "absolute")
"absolute"
my-cljs-projects.analog-clock=> (set! cvs.style.boxSizing "border-box")
"border-box"
my-cljs-projects.analog-clock=> (set! cvs.style.border "0")
"0"
my-cljs-projects.analog-clock=> (set! cvs.style.padding "0 0 0 0")
"0 0 0 0"
my-cljs-projects.analog-clock=> (set! cvs.style.margin "0 0 0 0")
"0 0 0 0"
;; キャンバスの描画コンテキストを取得
my-cljs-projects.analog-clock=> (def context (.getContext cvs "2d"))
#'my-cljs-projects.analog-clock/context
;; 描画の原点をキャンバスの中心にセット
my-cljs-projects.analog-clock=> (.translate context hankei hankei)
nil
;; キャンバスをHTMLに追加
my-cljs-projects.analog-clock=> (.appendChild view-elm cvs)
#object[HTMLCanvasElement [object HTMLCanvasElement]]
```

# REPL上での作業をatomベースに書き換えてみる

```
;; div要素を取得
my-cljs-projects.analog-clock=> (def view-elm (reagent/atom (gdom/getElement "app")))
#'my-cljs-projects.analog-clock/view-elm
;; イけるか確認
my-cljs-projects.analog-clock=> (.-clientWidth @view-elm)
817  ;; イける!!!
;; 時計の半径を取得
(defn- get-hankei [v]
  (let [tate (.-clientHeight @v)
        yoko (.-clientWidth @v)]
    (/ (min tate yoko) 2)))
my-cljs-projects.analog-clock=> (def hankei (get-hankei view-elm))
#'my-cljs-projects.analog-clock/hankei
;; div要素の縦横の短い方を取得
my-cljs-projects.analog-clock=> (def mini-side (min (.-clientWidth @view-elm) (.-clientHeight @view-elm)))
#'my-cljs-projects.analog-clock/mini-side
;; キャンバスの作成
my-cljs-projects.analog-clock=> (def cvs (reagent/atom (gdom/createElement "canvas")))
#'my-cljs-projects.analog-clock/cvs
;; キャンバスの描画サイズセット
my-cljs-projects.analog-clock=> (.-width @cvs)
300
my-cljs-projects.analog-clock=> (.setAttribute @cvs "width" mini-side)
nil
my-cljs-projects.analog-clock=> (.-width @cvs)
400
my-cljs-projects.analog-clock=> (.setAttribute @cvs "height" mini-side)
nil
;; キャンバスの表示サイズセット
my-cljs-projects.analog-clock=> (.-width (.-style @cvs))
""
my-cljs-projects.analog-clock=> (set! (.-width (.-style @cvs)) (str mini-side "px"))
"400px"
my-cljs-projects.analog-clock=> (.-width (.-style @cvs))
"400px"
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
my-cljs-projects.analog-clock=> (set-cvs-style cvs)
"0 0 0 0"
;; キャンバスの描画コンテキストを取得
my-cljs-projects.analog-clock=> (def context (reagent/atom (.getContext @cvs "2d")))
#'my-cljs-projects.analog-clock/context
my-cljs-projects.analog-clock=> @context
#object[CanvasRenderingContext2D [object CanvasRenderingContext2D]]
;; 描画の原点をキャンバスの中心にセット
my-cljs-projects.analog-clock=> (.translate @context hankei hankei)
nil
;; キャンバスをHTMLに追加
my-cljs-projects.analog-clock=> (.appendChild @view-elm @cvs)
#object[HTMLCanvasElement [object HTMLCanvasElement]]
```

ブラウザにキャンバスの要素が追加されていたので良しとする。

次は、これをソースファイルに記述して動作を確認してみる。