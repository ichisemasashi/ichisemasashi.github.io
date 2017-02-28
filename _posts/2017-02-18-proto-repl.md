---
layout: post
title: Proto REPL AtomエディタでのClojure開発環境
date:   2017-02-18
categories: Clojure
---
# Proto REPL : AtomエディタでのClojure開発環境

[Proto REPL](https://atom.io/packages/proto-repl)はClojure開発環境であり、AtomのREPLです。
機能のデモンストレーションについては、[proto-repl-demoプロジェクト](https://github.com/jasongilman/proto-repl-demo)を参照してください。

## 特徴

- インタラクティブなREPL主導の開発環境
- Clojureの名前空間、関数名、変数、ローカルバインディングの自動補完
- コードブロックまたは選択したコードをキーストロークで評価します。
- 結果をREPLまたはコードのすぐ近くにインラインで表示します。
- 入力時にファイル内のコードを実行する自動評価モード。
- ネームスペースまたはプロジェクト全体で簡単にテストを実行できます。
- リンクされたClojureライブラリの、ドキュメントとコードを表示します。
- REPLを制御できるAtom Tool Barの統合。
- 独自のコマンドを追加したり、ビジュアライゼーションを作成する機能を持つ拡張可能。


## セットアップ

[Atom Clojure Setup](https://git.io/atom_clojure_setup)

※ 詳細はリンク先を参照してください。

- Atomのインストール
- JavaとLeiningenのインストール
- パッケージのインストール
  - proto-repl, proto-repl-charts, ink, tool-bar, Parinfer, lisp-paredit, highlight-selected, set-syntax
- パッケージの設定
  - language-clojure, bracket-matcher, Proto REPL, lisp-paredit
- Atomの設定
  - Auto Indent On Paste, Scroll Past End
- styles.less
- init.coffeeとkeymap.cson


## 使い方
### ローカルにてClojure REPLを開始する
基本的に[Leiningen](https://leiningen.org/)や[Boot](http://boot-clj.com/)を使うプロジェクトで動作します。

1. ClojureのプロジェクトをAtomで開く。
2. REPLの開始は、コマンド・パレットから`Proto REPL: Toggle`を選択する。キーボード・ショートカットからも起動できます。

### リモートのREPLに接続する
Proto REPLは、nREPLを使用してリモートのClojureプロセスに接続できます。 コマンド・パレット（cmd-alt-p）を起動し、 `Proto REPL：Remote Nrepl Connection`を選択して、リモートREPLに接続します。 リモートnREPLサーバのホストとポートを入力すると、接続されます。 キーバインド`ctrl-alt-, y`も機能します。

### セルフホストなClojureScriptのREPLを開始する
Proto REPLには、セルフホストなClojureScriptのREPLを起動する機能が含まれています。 これは、ClojureScriptを使用してAtomエディタの内部で実行されるREPLです。 現在、その能力はかなり限られていますが、今後も改善され続けます。 Atom内でREPLを実行する機能は、Proto REPLを使用してProto REPLを開発し、ClojureScriptにProto REPLのコードを書きやすくします。 また、JavaやLeiningenやBootなどのビルドシステムをインストールして基本的な作業を行う必要がなくなります。

コマンド・パレットを起動し、 `Proto REPL：Start Self Hosted Repl`を選択して、セルフホストREPLを開始します。 キーバインド`ctrl-alt-,j`も機能します。

これは現在、セルフホストREPLでサポートされている機能のリストです。

- REPLからコマンドを実行する。
- ブロックや選択したコードの実行
- varsのオートコンプリート
- Proto REPLチャートを使用してチャートとグラフを描画します。
- 結果のインライン表示
- varのドキュメントを表示します。
- 自動評価モード。

これは、現在まだREPLホストされている自己でサポートされていない機能のリストです。

- 外部ファイルの読み込み。
- キャプチャされた値を保存し、表示します
- 実行時間の長いコマンドを中断。
- varソースコードを出力します。
- VARの定義を開きます。
- テストの実行。

### Leiningenプロジェクトの外での利用

Proto REPLは依然としてLeiningenプロジェクトの外でREPLを開始することができます。 Leiningenを使用してREPLを開始しますが、Proto REPLに同梱されているデフォルトのプロジェクトを使用します。 これにより、Clojureファイルや新しいAtomウィンドウを簡単に開くことができ、実験のために新しいREPLを起動することができます。

### REPLの中で入力する

REPLで実行されるコードは、最後の入力領域に入力することで入力できます。 Shift + Enterを押すとコードを実行できます。 REPLは実行履歴を保持します。 履歴は、カーソルをテキスト入力エリアに置いた後に上下の矢印キーを使用してナビゲートすることができます。

### REPLにコードを送る

REPL自体または他のテキストエディタからコードをREPLに送ることができます。 たとえば、MarkdownファイルにREPLにも送信できるClojureコードがある場合などです。

#### ブロックを送る

Clojureコードのブロックは、かっこ`()`、中括弧`{}`（Clojureでマップリテラルを定義する）、または角括弧`[]`（Clojureでベクトルリテラルを定義する）で区切られたコードです。 現在のテキストエディタからブロックを送信するには、キーバインド`ctrl-alt-,b`（ctrlとカンマを一緒に押す、リリースしてからbを押す）を使用できます。 送信されるブロックは、カーソルの位置によって異なります。 カーソルは、複数のブロックの内側、ブロックの直後、またはブロックの前にネストされていてもよい。 ブロック発見のための論理は、以下の順序でブロックを探索する。

1.  カーソルの直後のブロック。
1.  カーソルの直前のブロック。
1.  カーソルがネストされている最初のブロック。

例：次の例は、Clojureコードのサンプルを示しています。 `|`はカーソル位置を示す。

|Code	| Code sent to REPL|	Why?|
|`|(foo 1 2)`| (foo 1 2)|ブロック直前のカーソル|
|`(foo 1 2)|`| (foo 1 2)|ブロック直後のカーソル|
|`(a (b |(c (foo 1 2))))`| (c (foo 1 2))| cブロック直前のカーソル|
|`(a (b| (c (foo 1 2))))`| (b (c (foo 1 2)))|bブロック内のカーソル Cursor inside b block|

##### Markdownブロック

ブロック検出では、Github Flavored Markdown Clojureブロックの開始と終了を見つけることもできます。 カーソルがClojureブロックの外にあるがMarkdown Clojureブロック内にある場合、Markdownブロック内のすべてのコードが送信されます。

#### 選択したコードを送る

選択されたClojureコードの任意のセットは、コードを選択し、キーバインディングc`trl-alt-,s`を使用してREPLに送信できます（ctrlとカンマを一緒に押して、リリースしてからsキーを押します）。 これにより、一度に複数のコードブロックを送信することができます。

### 自動補完

Proto REPLは、Complimentライブラリを使用して、名前空間、vars、関数、ローカルバインディング、およびJavaメソッドの完成をサポートしています。 プロジェクトに依存関係として`[proto-repl "0.3.1"]`が含まれていることを確認してください。

ヒント

REPLを開始する必要があります
必要な名前空間の定義を変更した後、それを再評価して、補完がエイリアスの変更を受け取れるようにします。

### インラインでの結果表示

 Atom Inkパッケージがインストールされている場合、実行されたブロックまたは選択項目のインライン表示がサポートされています。 configurationを使用してインライン結果を無効にすることができます。 インラインで表示された値は、すべてのデータを表示することなく大きなネストされたデータ構造を探索できるようなツリー形式で表示されます。

### 自動評価モード

（自動評価はベータ版であり、変更される可能性があります）[改善のための問題や提案を報告してください](https://github.com/jasongilman/proto-repl/issues)。

Proto REPLは入力時に最上位レベルのフォームの自動評価をサポートします。 結果は各Top Level フォームの横にインラインで表示されます。 これには、 Atom Inkをインストールする必要があります。 Atomコマンド・パレット（cmd-alt-p）をトグルし、「Proto Repl：Autoeval File」を選択することにより、ファイルの自動評価モードを開始することができます。 Atomコマンド・パレット（cmd-alt-p）をトグルし、「Proto Repl: Stop Autoeval File」を選択すると、停止できます。


表示された可視化は、Proto REPL Chartsで作成されました。

### ローカル バインディング値の保存と閲覧

Clojureの関数またはletブロックの中に値があるシンボルがあります。 この例では、`m`、`a`、および`b`を合計するコードはすべてローカルバインディングです。

```
(reduce (fn [m [a b]]
         (update m a #(+ b (or % 0))))
       {}
       [[:apples 2] [:oranges 3] [:apples 4] [:cherries 7]])
=>
{:apples 6, :oranges 3, :cherries 7}
```

このコードは単純ですが、関数やループ内で何が起こっているのかを理解することは難しいでしょう。 この種のコードをデバッグするために、多くの開発者がロギングや印刷を行います。 これを複数の関数と名前空間にわたって実行すると、これらの値は混在してコードから分離されます。 ローカルバインディングを保存して表示するためのProto REPLの新機能では、コンテキストや複数の要求から値を見ることができます。


次のコードは`(proto-repl.saved-values/save 1)`以外は前と同じです。 `proto-repl.saved-values/save`関数は、すべてのローカルバインディングを保存して、Proto REPLで表示できるようにします。  `(proto-repl.saved-values/save 1)`の`1`は保存された値を表示するためにProto REPLに戻すためのユニークなIDです。

```
(reduce (fn [m [a b]]
         (proto-repl.saved-values/save 1)
         (update m a #(+ b (or % 0))))
       {}
       [[:apples 2] [:oranges 3] [:apples 4] [:cherries 7]])
=>
{:apples 6, :oranges 3, :cherries 7}
```

`proto-repl:display-saved-value`コマンドを呼び出すコードを実行すると、値がテーブルに表示されます。 表の各行は、関数の異なる繰り返しを表します。

テーブルは、表示可能な詳細の量が制限されています。 Proto REPLは長いClojureデータ構造を列に収めるように切り詰めます。 表の各行を拡張して、大規模なデータ構造の詳細を調べることができます。

保存する特定のバインディングを指定することもできます。 例えば`(proto-repl.saved-values/save 1 m a)` は、ローカル変数`m`と`b`の値だけを保存します。

#### Defボタン - 保存されたローカルバインディングのvarの定義

保存された値テーブルに表示されている "def"ボタンを使用すると、ローカルバインディングと同じ名前で名前空間内の変数を一時的に定義することができます。 これにより、保存された値を使用してコードを簡単に試すことができます。 コードのビット部分を簡単に再評価することができ、すべてのローカルバインディングを使用できるようになります。


#### セーブバリュー機能の使用

1. `proto-repl.saved-values/save`への呼び出しを、キーバインディング`ctrl-alt-shift-,i`（Ctrlキーを押しながらカンマを押してから、iを押す）を使用してコードに挿入すると、一意の番号の保存呼び出しが挿入されます。 一意の番号を使用すると、コード内の別の場所に複数の保存呼び出しを行うことができます。
1. コードを実行します。 関数内または複数の名前空間にコードを配置した場合は、コードを実行する前に、変更したコードを再定義するかリフレッシュする必要があります。
1. `ctrl-alt-shift-,d` キーバインドを押して値を表示する
1. 保存された値は、キーバインド`ctrl-alt-shift-,c`

現在のところ、proto-repl-libには20個の保存された値の制限があります。 デバッグ後に問題があれば、必ずセーブコールを削除してください。 彼らはローカル開発にのみ使用されることを意図しています。

## Proto REPLの拡張

[proto-repl/extending_proto_repl.md](https://github.com/jasongilman/proto-repl/blob/master/extending_proto_repl.md)

詳細はリンク先を参照してください。

- イベントにサブスクライブ
- 新しいREPLコマンドを定義
- 選択されたテキストの上でREPLコマンドを定義
- コード実行の拡張
- 例


## キーバインドとイベント

|キーバインド|イベント|動作|
|`ctrl-alt-, L`|`proto-repl:toggle`|REPL開始する|
|`ctrl-alt-, shift-L`|`proto-repl:toggle`|現在開いているproject.cljをつかってREPLを開始する|
|`ctrl-alt-, y`|`proto-repl:remote-nrepl-connection`|遠隔のnREPLセッションと接続する|
|`ctrl-alt-, j`|`proto-repl:start-self-hosted-repl`|セルフホストREPLを開始する|
|`ctrl-alt-, e`|`proto-repl:exit-repl`|REPLを抜ける|
|`ctrl-alt-, k`|`proto-repl:clear-repl`|REPL出力をクリアする|
|`ctrl-alt-shift-, s`|`proto-repl:toggle-auto-scroll`|REPLの自動スクロールをOn/OFFする|
|`ctrl-alt-, b`|`proto-repl:execute-block`|実行するためにClojureコードの現在のブロックをREPLに送信します。|
|`ctrl-alt-, B`|`proto-repl:execute-top-block`|実行するためにClojureコードの現在のトップレベルブロックをREPLに送信します。|
|`ctrl-alt-, s`|`proto-repl:execute-selected-text`|選択されたテキストをREPLに送信して実行します。|
|`ctrl-alt-, f`|`proto-repl:load-current-file`|現在のファイルをREPLにロードする|
|`ctrl-alt-, r`|`proto-repl:refresh-namespaces`|user/reset機能を実行します。 [My Clojure Workflow、Reloaded](http://thinkrelevance.com/blog/2013/06/04/clojure-workflow-reloaded)を参照してください。|
|`ctrl-alt-shift-, r`|`proto-repl:super-refresh-namespaces`|`clojure.tools.namespace`を使用してロードされたすべての名前空間をクリアします.`user/reset`関数が実行されます。|
|`ctrl-alt-, p`|`proto-repl:pretty-print`|REPLで返された最後の値をプリティープリントします。|
|`ctrl-alt-, x`|`proto-repl:run-tests-in-namespace`|カレント名前空間でのテストを全て実行する|
|`ctrl-alt-, t`|`proto-repl:run-test-under-cursor`|カーソルの下に名前があるテストを実行します。|
|`ctrl-alt-, a`|`proto-repl:run-all-tests`|現在のプジョジェクト内の全てのテストを実行する|
|`ctrl-alt-, d`|`proto-repl:print-var-documentation`|varのドキュメントをカーソルの下に表示します。|
|`ctrl-alt-, c`|`proto-repl:print-var-code`|カーソルの下にあるvarのコードを出力します。|
|`ctrl-alt-, o`|`proto-repl:open-file-containing-var`|カーソルの下のvarまたは名前空間のコードを開きます。 これはライブラリで定義された変数でも機能します。|
|`ctrl-alt-, n`|`proto-repl:list-ns-vars`|カーソルの下の名前空間の変数をリストします。|
|`ctrl-alt-shift-, n`|`proto-repl:list-ns-vars-with-docs`|ドキュメントのあるカーソルの下の名前空間の変数をリストします。|
|`shift-ctrl-c`|`proto-repl:interrupt`|REPLで現在実行中のコマンドを中断しようとします。|
|`ctrl-alt-shift-, i`|`proto-repl:insert-save-value-call`|一意のIDを持つ`proto/save`への呼び出しを挿入します。|
|`ctrl-alt-shift-, d`|`proto-repl:display-saved-values`|`proto/save`関数を使用して保存された値を表示します。|
|`ctrl-alt-shift-, c`|`proto-repl:clear-saved-values`|`proto/save`関数を使用して、以前に保存した値をクリアします。|
