---
layout: post
title: Analog時計11
date:   2021-09-20
categories: ClojureScript ブログ
---

# プロダクション・ビルド

[参考](http://ichisemasashi.github.io/clojure/%E5%8B%9D%E6%89%8B%E7%BF%BB%E8%A8%B3/2021/06/02/read-blog.html)

コンパイル方法は、以下

```
clj -M -m cljs.main --optimizations advanced -c my-cljs-projects.analog-clock
```

出力されたファイルを確認

```
% ls -l out/*.js
-rwxrwxrwx  1 ichise  staff  102228 Sep 20 19:42 out/main.js*
```

resources/public/index.htmlを書き換えて、プロジェクトフォルダの直下にコピー。(参考用ファイルは退避)
```html
% git diff
diff --git a/analog-clock/resources/public/index.html b/analog-clock/resources/public/index.html
index 76bbcfd..b2a5bf9 100644
--- a/analog-clock/resources/public/index.html
+++ b/analog-clock/resources/public/index.html
@@ -9,6 +9,6 @@
   <body>
     <div id="app">
     </div> <!-- end of app div -->
-    <script src="cljs-out/dev-main.js" type="text/javascript"></script>
+    <script src="out/main.js" type="text/javascript"></script>
   </body>
 </html>
```
動作確認

```
clj -M -m cljs.main --serve
```

`http://localhost:9000`にブラウザでアクセスすると、真っ白。インスペクトするとキャンバス要素はあるが、サイズが0pxになっている。最適化されすぎたのか!?


## コンパイルから遣り直してみる。

```
clj -M -m cljs.main --optimizations simple -c my-cljs-projects.analog-clock
```

こんどはキャンバス要素すらなくなった。

## コンパイルから遣り直してみる。

```
clj -M -m cljs.main --optimizations none -c my-cljs-projects.analog-clock
```

こんどは時計の表示はないが、キャンバス要素があり、1秒ごとにエラーが出ているので、時計っぽい動きにはなっている。

## コンパイルから遣り直してみる。

```
% clj -A:fig:build                         
```

REPLが開始しない、かつ、ブラウザに時計が表示されない。

## index.htmlを書き戻してコンパイルの遣り直し。

```
% clj -A:fig:build                         
```

ブラウザにアナログ時計が表示された。

## もういちど

```
% clj -M -m cljs.main --optimizations advanced -c my-cljs-projects.analog-clock
```

やっぱり、ブラウザに時計が表示されず、キャンバス要素が無い。


##

- プロジェクト直下のindex.htmlを修正。
- 最適化`none`でコンパイル(JSの生成)
- `clj -M -m cljs.main --serve`でサーバー起動
- ブラウザを見ると、キャンバスのwidthとheightが0。
- 親要素のdivのサイズが`817x0`
- body要素も`0`