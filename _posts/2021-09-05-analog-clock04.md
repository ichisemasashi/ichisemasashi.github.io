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