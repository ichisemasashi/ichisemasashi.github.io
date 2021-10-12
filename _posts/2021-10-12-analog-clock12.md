---
layout: post
title: Analog時計12
date:   2021-10-12
categories: ClojureScript ブログ
---

# やっと動いた

canvas要素がサイズ0になって久しいですが、ようやく気付きました。

ファイル「style.css」で強引にサイズ指定していたことに気付き、なぜCSSが反映されていないか確認してみました。

index.htmlの位置を動かしていたため、cssファイルの相対パスがズれていました。それを直したら動きました。


```
clj -M -m cljs.main --optimizations none -c my-cljs-projects.analog-clock
```

これで動きました。
