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

Code to be executed in the REPL can be entered by typing in the last entry area. Code can be executed by pressing shift+enter. The REPL maintains a history of executed commands that were entered in the REPL. The history can be navigated by using the up and down arrow keys after placing the cursor in the text entry area.

REPLで実行されるコードは、最後の入力領域に入力することで入力できます。 Shift + Enterを押すとコードを実行できます。 REPLは、REPLに入力された実行されたコマンドの履歴を保持します。 履歴は、カーソルをテキスト入力エリアに置いた後に上下の矢印キーを使用してナビゲートすることができます。

### REPLにコードを送る

Code can be sent to the REPL from within the REPL itself or any other open text editor. For example if you have some Clojure code in a Markdown file that can be sent to the REPL as well.

コードは、REPL自体または他のオープンテキストエディタ内からREPLに送ることができます。 たとえば、マークアップファイルにREPLにも送信できるClojureコードがある場合などです。

#### ブロックを送る
#### Markdownブロック
#### 選択したコードを送る
### 自動補完
### インラインでの結果表示
### 自動評価モード
### ローカル バインディング値の保存と閲覧
## Proto REPLの拡張
## キーバインドとイベント
