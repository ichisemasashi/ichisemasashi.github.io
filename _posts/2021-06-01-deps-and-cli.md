---
layout: post
title: Deps and CLI Guide
date:   2021-06-01
categories: Clojure, 勝手翻訳
---
# DepsとCLJのガイド



Clojureには、以下のためのコマンドラインツールがあります。

- インタラクティブな REPL (Read-Eval-Print Loop)の実行
- Clojureプログラムの実行
- Clojureの式を評価する

上記のすべてのシナリオにおいて、他のClojureやJavaのライブラリ(依存関係(dependencies)または「deps」)を使用したい場合があります。これらは、ローカルで作成しているライブラリ、git内のプロジェクト(例えばGitHub)、または一般的にはMavenエコシステムで利用可能なライブラリで、Maven CentralやClojarsのような中央リポジトリでホストされているものかもしれません。

いずれの場合も、ライブラリの使用には以下が必要です。

1. 使用したいライブラリを指定し、その名前とバージョンなどのその他の情報を提供する
2. git や maven リポジトリからローカルマシンにライブラリを（一度だけ）取得する。
3. REPLやプログラムの実行中にClojureが見つけられるように、JVMのクラスパスで利用できるようにする。

Clojureツールは、(1)のための構文とファイル(deps.edn)を指定し、(2)と(3)を自動的に処理します。

ツールのインストール方法については，[Getting Started](https://clojure.org/guides/getting_started)を参照してください。ここでは，その方法を説明します。完全なリファレンスについては、[Deps and CLI](https://clojure.org/reference/deps_and_cli)を参照してください。バージョン情報は[changelog](https://github.com/clojure/brew-install/blob/1.10.3/CHANGELOG.md)をご覧ください。

## REPLの実行とライブラリの使用

ツールをダウンロードしてインストールしたら、cljツールを実行してREPLを開始できます。

```bash
$ clj
Clojure 1.10.1
user=>
```

REPLでは、Clojureの式を入力してEnterキーを押すと、その式を評価することができます。REPLを終了するにはControl-Dを押します。


```bash
user=> (+ 2 3)   # 式を入力した後に `enter` キーを押すと、式が評価されます。
5                # 式の結果
user=>           # ここでCtrl-Dを押すと、REPLを終了します（表示はされません）。
$
```

ClojureやJavaのライブラリには、必要と思われる機能に実質的にアクセスできるものがたくさんあります。例えば、日付や時刻を扱うためによく使われるClojureライブラリ[clojure.java-time](https://github.com/dm3/clojure.java-time)を考えてみましょう。

このライブラリを使用するには、依存関係を宣言する必要があります。これにより、ツールはライブラリがダウンロードされたことを確認し、クラスパスに追加することができます。ほとんどのプロジェクトのReadmeには、使用するライブラリの名前とバージョンが記載されています。依存関係を宣言するために、`deps.edn`ファイルを作成します。

```Clojure
{:deps
 {clojure.java-time/clojure.java-time {:mvn/version "0.3.2"}}}
```

`clj`ツールを使ってREPLを再起動しましょう。

```
$ clj
Downloading: clojure/java-time/clojure.java-time/0.3.2/clojure.java-time-0.3.2.pom from clojars
Downloading: clojure/java-time/clojure.java-time/0.3.2/clojure.java-time-0.3.2.jar from clojars
Clojure 1.10.1
user=> (require '[java-time :as t])
nil
user=> (str (t/instant))
"2020-09-01T03:42:47.691119Z"
```

依存関係を初めて使用する際に、ライブラリのダウンロードに関するメッセージが表示されます。一度ダウンロードされたファイルは、今後も再利用されます。同じプロセスで、他のライブラリを`deps.edn`ファイルに追加したり、ClojureやJavaのライブラリを調べたりすることができます。

## プログラムの作成

すぐに、これらのライブラリを利用した独自のコードをビルドして保存したくなるでしょう。新しいディレクトリを作成し、この`deps.edn`をそこにコピーします。

```bash
$ mkdir hello-world
$ cp deps.edn hello-world
$ cd hello-world
$ mkdir src
```

デフォルトでは、`clj`ツールは`src`ディレクトリにあるソースファイルを探します。`src/hello.clj`を作成します。

```Clojure
(ns hello
  (:require [java-time :as t]))

(defn time-str
  "Returns a string representation of a datetime in the local time zone."
  [instant]
  (t/format
    (t/with-zone (t/formatter "hh:mm a") (t/zone-id))
    instant))

(defn run [opts]
  (println "Hello world, the time is" (time-str (t/instant))))
```

## mainの使用

このプログラムには、エントリー関数`run`があり、`clj`で`-X`を使って実行することができます。

```bash
$ clj -X hello/run
Hello world, the time is 10:53 PM
```

## ローカルライブラリの利用

このアプリケーションの一部をライブラリに移すことになるかもしれません。`clj`ツールは、ローカルディスク上にのみ存在するプロジェクトをサポートするために、ローカル座標を使用します。このアプリケーションのjava-timeの部分を、並列ディレクトリtime-libのライブラリに取り出してみましょう。最終的な構造は以下のようになります。

```bash
├── time-lib
│   ├── deps.edn
│   └── src
│       └── hello_time.clj
└── hello-world
    ├── deps.edn
    └── src
        └── hello.clj
```


time-libでは、すでにあるdeps.ednファイルのコピーを使い、`src/hello_time.clj`というファイルを作成します。

```Clojure
(ns hello-time
  (:require [java-time :as t]))

(defn now
  "Returns the current datetime"
  []
  (t/instant))

(defn time-str
  "Returns a string representation of a datetime in the local time zone."
  [instant]
  (t/format
    (t/with-zone (t/formatter "hh:mm a") (t/zone-id))
    instant))
```

`hello-world/src/hello.clj`のアプリケーションを更新して、代わりにあなたのライブラリを使うようにします。

```Clojure
(ns hello
  (:require [hello-time :as ht]))

(defn run [opts]
  (println "Hello world, the time is" (ht/time-str (ht/now))))
```

`hello-world/deps.edn`を修正し、time-libライブラリのルートディレクトリを参照するローカル座標を使用するようにします（ご使用のマシンに合わせてパスを更新してください）。

```Clojure
{:deps
 {time-lib/time-lib {:local/root "../time-lib"}}}
```

その後、アプリケーションを実行することで、hello-worldディレクトリのすべてをテストすることができます。

```bash
$ clj -X hello/run
Hello world, the time is 02:07 PM
```

## gitライブラリの利用

そのライブラリを他の人と共有できたらいいですよね。これを実現するには、プロジェクトを公開または非公開のgitリポジトリにプッシュし、git dependency coordinateを使って他の人に使ってもらうようにします。

まず、time-lib用のgitライブラリを作成します。

```bash
cd ../time-lib
git init
git add deps.edn src
git commit -m 'init'
```

次に、（GitHub のような）パブリックな git リポジトリホストに行き、この git リポジトリを作成して公開するための指示に従います。

最後に、アプリを変更してgit dependencyを使用するようにします。そのためには、次のような情報を集める必要があります。

- repository url - GitHub では、https://github.com/yourname/time-lib.git のような HTTPS url を使用します。
- sha - 使用したい git ライブラリのバージョンを指定します。git rev-parse HEAD を実行すると、現在のレポの sha を得ることができます。

`hello-world/deps.edn`を更新して、代わりにgit座標を使うようにします。

```Clojure
{:deps
 {github-yourname/time-lib
  {:git/url "https://github.com/yourname/time-lib" :sha "04d2744549214b5cba04002b6875bdf59f9d88b6"}}}
```

ライブラリ名を変更していることに注意してください。アーティファクトをMavenリポジトリにデプロイする際には、(通常はDNSや商標を介して)自分が管理しているものであるgroupId(名前の最初の部分)を使用するのがベスト・プラクティスです。どちらも持っていない場合は、GitHub のような ID を確立するサイトの名前とそのサイトでの自分の ID を組み合わせて、`github-yourname` とすることができます。

これで、アプリを再び実行して (共有の) git リポジトリライブラリを利用できるようになりました。初回の実行時には、`clj` がリポジトリとコミット作業ツリーをダウンロードしてキャッシュする際に、コンソールに追加のメッセージが表示されます。

```
$ clj -X hello/run
Cloning: https://github.com/yourname/time-lib
Checking out: https://github.com/yourname/time-lib at 04d2744
Hello world, the time is 02:10 PM
```

これであなたの友だちも`time-libを利用できるようになりました。

## その他の例

プログラムが複雑になってくると、標準的なクラスパスのバリエーションを作成する必要があるかもしれません。Clojureツールは、エイリアスを使ったクラスパスの変更をサポートしています。エイリアスとは、対応するエイリアスが与えられたときにのみ使用されるdepsファイルの一部です。できることの一部を紹介します。

- テストソースディレクトリを含める
- すべてのテストの実行にテストランナーを使用する
- オプションの依存関係の追加
- 依存関係のバージョンを上書きする
- ディスク上のローカル jar を使用する
- AOT（Ahead-of-Time）コンパイル
- ソケットサーバーのリモートREPLの実行

### テストソースディレクトリを含める

通常、プロジェクトのクラスパスには、プロジェクトのソースのみが含まれ、デフォルトではテストソースは含まれません。クラスパス構築のmake-classpathステップで、プライマリクラスパスに修正として余分なパスを追加することができます。そのためには、`"test"`という追加の相対ソースパスを含むエイリアス `:test` を追加します。

```Clojure
{:deps
 {org.clojure/core.async {:mvn/version "1.3.610"}}

 :aliases
 {:test {:extra-paths ["test"]}}}
```

そのクラスパスの修正を適用し、`clj -A:test -Spath` を起動して修正後のクラスパスを調べます。

```bash
$ clj -A:test -Spath
test:
src:
/Users/me/.m2/repository/org/clojure/clojure/1.10.1/clojure-1.10.1.jar:
... same as before (split here for readability)
```

test dirがクラスパスに含まれるようになったことに注意してください。


### すべてのテストの実行にテストランナーを使用する

前のセクションの `:test` エイリアスを拡張して、すべての clojure.test テストを実行する cognitect-labs [test-runner](https://github.com/cognitect-labs/test-runner) を含めることができます。

`:test`エイリアスを拡張します。

```Clojure
{:deps
 {org.clojure/core.async {:mvn/version "1.3.610"}}

 :aliases
 {:test {:extra-paths ["test"]
         :extra-deps {io.github.cognitect-labs/test-runner
                      {:git/url "https://github.com/cognitect-labs/test-runner.git"
                       :sha "9e35c979860c75555adaff7600070c60004a0f44"}}
         :main-opts ["-m" "cognitect.test-runner"]
         :exec-fn cognitect.test-runner.api/test}}}
```

そして、デフォルトの設定でテストランナーを実行します（test/ディレクトリの下の-test名前空間ですべてのテストを実行します）。

```
clj -X:test
```

### オプションの依存関係の追加

`deps.edn`ファイルのエイリアスを使って、クラスパスに影響を与えるオプションの依存関係を追加することもできます。

```Clojure
{:aliases
 {:bench {:extra-deps {criterium/criterium {:mvn/version "0.4.4"}}}}}
```

ここでは、`:bench`エイリアスを使って、追加の依存関係、つまりcriteriumベンチマークライブラリを追加しています。

この依存関係をクラスパスに追加するには、`:bench`エイリアスを追加して依存関係の解決方法を変更します：`clj -A:bench`.

### 依存関係のバージョンを上書きする

複数のエイリアスを組み合わせて使用することができます。例えば、この`deps.edn`ファイルでは、古いバージョンの`core.async`を強制的に使用するための`:old-async`と、追加の依存関係を追加するための`:bench`という2つのエイリアスを定義しています。

```Clojure
{:deps
 {org.clojure/core.async {:mvn/version "0.3.465"}}

 :aliases
 {:old-async {:override-deps {org.clojure/core.async {:mvn/version "0.3.426"}}}
  :bench {:extra-deps {criterium/criterium {:mvn/version "0.4.4"}}}}}
```

`clj -A:bench:old-async`のように、両方のエイリアスを有効にします。

### ディスク上のローカル jar を使用する

場合によっては、データベースドライバのjarなど、Mavenリポジトリに存在しないディスク上のjarを直接参照する必要があるかもしれません。

ローカルのjarの依存関係を、ディレクトリではなくjarファイルを直接指すローカル座標で指定します。

```Clojure
{:deps
 {db/driver {:local/root "/path/to/db/driver.jar"}}}
```

### AOT（Ahead-of-Time）コンパイル

[gen-class](https://clojure.github.io/clojure/clojure.core-api.html#clojure.core/gen-class)または[gen-interface](https://clojure.github.io/clojure/clojure.core-api.html#clojure.core/gen-interface)を使用する場合、javaクラスを生成するために、Clojureソースを事前にコンパイルする必要があります。

これは、`compile`を呼び出すことで行うことができます。コンパイルされたクラスファイルのデフォルトの保存先は `classes/` で、これを作成してクラスパスに追加する必要があります。

```bash
$ mkdir classes
```

`deps.edn`を編集して、パスに`"classes"`を追加します。

```Clojure
{:paths ["src" "classes"]}
```

src/my_class.cljでgen-classを使ってクラスを宣言します。

```Clojure
(ns my-class)

(gen-class
  :name my_class.MyClass
  :methods [[hello [] String]])

(defn -hello [this]
  "Hello, World!")
```

そして、別のソースファイル`src/hello.clj`の中で、`:import`を使ってクラスを参照することができます。名前空間は`:require`でも追加されているので、コンパイル時に自動的に依存するすべての名前空間を見つけてコンパイルできることに注意してください。

```Clojure
(ns hello
  (:require [my-class])
  (:import (my_class MyClass)))

(defn -main [& args]
  (let [inst (MyClass.)]
    (println (.hello inst))))
```

REPLでコンパイルすることも、スクリプトを実行してコンパイルを行うこともできます。

```
$ clj -M -e "(compile 'hello)"
```

そして、hello名前空間を実行します。

```
$ clj -M -m hello
Hello, World!
```

完全なリファレンスは[Compilation and Class Generation](https://clojure.org/reference/compilation)を参照のこと。

### ソケットサーバーのリモートREPLの実行

Clojureには、ソケットサーバーを実行するためのサポートが組み込まれており、特にリモートREPLをホストするために使用します。

ソケットサーバー repl を設定するには、以下の基本設定を `deps.edn` に追加します。

```Clojure
{:aliases
 {:repl-server
  {:exec-fn clojure.core.server/start-server
   :exec-args {:name "repl-server"
               :port 5555
               :accept clojure.core.server/repl
               :server-daemon false}}}}
```

そして、エイリアスで起動してサーバーを起動します。

```
clojure -X:repl-server
```

必要に応じて、コマンドラインでデフォルトのパラメータを上書きしたり、オプションを追加したりすることもできます。

```
clojure -X:repl-server :port 51234
```

netcatを使って別の端末から接続することができます。

```
nc localhost 51234
user=> (+ 1 1)
2
```

Ctrl-Dでレプリを終了し、Ctrl-Cでサーバーを終了します。

原著者 アレックス・ミラー


www.DeepL.com/Translator（無料版）で翻訳しました。