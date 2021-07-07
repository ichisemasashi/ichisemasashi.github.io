---
layout: post
title: はじめてのClojureScript
date:   2021-07-05
categories: ClojureScript ブログ
---

# はじめてのClojureScript

参考: Learn ClojureScript ch05

## ディレクトリ構成
まずはプロジェクトのディレクトリを作成。

```
% mkdir first-project
% cd first-project 
```
※ プロジェクト名にはハイフンを使える。

つぎに、ソースの格納先と生成物(JavaScript)の格納先を作成。

```
% mkdir src
% mkdir src/first_project
% mkdir out
```
※ ディレクトリ名にはハイフンの代りにアンダーバーを使う。

## ファイルの作成
### deps.ednの作成。

```
% cat > deps.edn
{:deps {org.clojure/clojurescript {:mvn/version "1.10.866"}}
 :paths ["src"]}
^C
```

### ソースファイルの作成。

```
% cat src/first_project/core.cljs
;; FILE NAME: src/first_project/core.cljs
(ns first-project.core) ;; 名前空間の宣言

(js/alert "Hello world")
```

### index.htmlの作成

```
% cat > index.html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
  </head>
  <body>
    <script src="out/main.js" type="text/javascript"></script>
  </body>
</html>
^C
```

## 実行

```
% clj -m cljs.main --compile first-project.core --repl
WARNING: Use of -A with clojure.main is deprecated, use -M instead
ClojureScript 1.10.866
cljs.user=> 
```
- `-m cljs.main`フラグは、ClojureScriptコンパイラを実行するための指定。
- `--compile`フラグの後ろにはエントリーポイントの名前空間を指定する。
- `--repl`フラグはWebサーバーの起動と、REPLの起動をする。

## エイリアス

deps.ednにエイリアスの設定を追加する。

```
% cat deps.edn 
{:deps {org.clojure/clojurescript {:mvn/version "1.10.866"}}
 :paths ["src"]
 :aliases {:dev {:main-opts ["-m" "cljs.main"
                             "--compile" "first-project.core"
                             "--repl"]}}}
```

これで簡単に起動できる。

```
% clj -M:dev
ClojureScript 1.10.866
cljs.user=> 
```