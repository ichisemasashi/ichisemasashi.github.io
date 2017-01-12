---
layout: post
title: Clojureにおける変数(数値の場合)
date:   2017-01-13
categories: Clojure
---
# Clojureでの変数(数値の場合)

## 変数の作成
```
user=> (def my-foo (atom 0))
```

## 変数に演算子を作用させる
```
user=> (defn my-swap [] (swap! my-foo inc))
#'user/my-swap
user=> (my-swap)
4
```

## 変数の値の入れ替え
```
user=> (defn my-reset [n] (reset! my-foo n))
#'user/my-reset
user=> @my-foo
0
user=> (my-reset 3)
3
user=> @my-foo
3
```
