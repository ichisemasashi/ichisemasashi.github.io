---
layout: post
title: HaLake.cljの準備
date:   2018-03-10
categories: Clojure
---
# 久しぶりにHaLake.cljに参加するための下準備

## Leiningen：簡単にClojureを扱うためのツール

[Leiningenの本家サイト](https://leiningen.org/)

[Leiningenのチュートリアル](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md)

### ヘルプを読む
```
lein help
lein help $TASK
```
### Creating a Projec

(app templateを利用する例)
```
$ lein new app my-stuff
```

appを省略すると、default templateになる。

### Directory Layout
- プロジェクトのREADME
- コードを含むsrc/
- テスト用のtest/
- プロジェクトについての記述があるproject.clj
- ファイル「src/my_stuff/core.clj」は、ネームスペース「my-stuff.core」と対応している

### Dependencies

#### Artifact IDs, Groups, and Versions
利用したいライブラリのバージョンをインターネット経由で調べる
```
lein search $TERM
```
`project.clj`の`:dependencies`ベクタに記述

```
:dependencies [[org.clojure/clojure "1.8.0"]
[clj-http "2.0.0"]]
```

#### project.cljの設定をプロジェクトに反映する
```
$ lein install
```

### Setting JVM Options
どうやらJVMのコンパイル・オプションがあるらしい。たぶんproject.cljにでも設定するのだろう。

### Running Code
```
$ lein repl
....
user=>
```


````
user=> (require 'my-stuff.core)
nil
user=> (my-stuff.core/-main)
Hello, World!
nil
user=> (require '[clj-http.client :as http])
nil
user=> (def response (http/get "https://leiningen.org"))
#'user/response
user=> (keys response)
(:status :headers :body :request-time :trace-redirects :orig-content-encoding)
````

### -mainはコマンドラインから実行できる
```
$ lein run
Hello, World!
```

### Uberjar
```
$ lein uberjar
Compiling my-stuff.core
Created /home/phil/my-stuff/target/uberjar+uberjar/my-stuff-0.1.0-SNAPSHOT.jar
Created /home/phil/my-stuff/target/uberjar/my-stuff-0.1.0-SNAPSHOT-standalone.jar

$ java -jar my-stuff-0.1.0-SNAPSHOT-standalone.jar Hello world.
Welcome to my project! These are your args: (Hello world.)
```


[CIDERの本家サイト](https://cider.readthedocs.io/en/latest/)

### ciderの起動
```
M-x cider-jack-in RET
```

ciderが古いというエラーが表示された。最新のciderをインストールしようとしたらemacsのバージョンが古いという表示が出てしまった。
とりあえず、homebrewではないemacsをインストールする。
新しいemacsで、ciderをインストールするが、動作がおかしい。
よって、今回は、ciderを断念することにする。
