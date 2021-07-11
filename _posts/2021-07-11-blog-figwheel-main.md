---
layout: post
title: Figwheel-mainの威力
date:   2021-07-11
categories: ClojureScript ブログ
---

# Figwheel-mainでプロジェクトを起動

参考：https://github.com/ichisemasashi/clojure_books/blob/master/Learn_ClojureScript/001_06.md

基本的な設定は先日の設定。

`clj-new`でプロジェクトを生成するときにFigwheelをテンプレートとして指定(`:template figwheel-main`)した。

`cd`でプロジェクトのディレクトリへ移動して、
```
$ clj -A:fig:build
[Figwheel] Validating figwheel-main.edn
[Figwheel] figwheel-main.edn is valid \(ツ)/
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 9.014 seconds.
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

これで、ブラウザも立ち上がる。

Figwheel-mainは、ソースコードの監視をし、ソースコードの変更を検出するとそれをJavaScriptにコンパイルして、コンパイル結果のJavaScriptをブラウザに送信して実行する。

ソースファイルの関数 hello-world を変更してみる。
```
(defn hello-world []
  [:div
   [:h1 (:text @app-state)]
   [:h3 "Edit this in src/learn_cljs/weather.cljs and watch it change!"]])
```
これを以下に変更して保存すると、
```
(defn hello-world []
  [:div
    [:h1 "I say: " (:text @app-state)]])
```

すると、REPL上では以下のように表示され、ブラウザの表示も変更される。
```
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 0.7 seconds.
[Figwheel] Outputting main file: target/public/cljs-out/dev-main-auto-testing.js
This text is printed from src/learn_cljs/weather.cljs. Go ahead and edit it and see reloading in action.
```

# ただし、Figwheel-mainで変更できないものもある

defonceで宣言されたもの(app-state)への更新は変更されない。
```Clojure
(defonce app-state (atom {:text "Hello world!"}))
```

# Figwheel-mainはJavaScriptだけではない

Figwhell-mainは、JavaScriptのコンパイル、送信、実行だけでなく、変更する可能性のあるスタイルシートのリロードも行う。

たとえば、`resources/public/css/style.css`を以下のように変更すると、ファイルが保存されたときにブラウザにもその変更が伝わる。
```CSS
body {
  background-color: #02a4ff;
  color: #ffffff;
}

h1 {
  font-family: Helvetica, Arial, sans-serif;
  font-weight: 300;
}
```