---
layout: post
title: Proto REPL : AtomエディタでのClojure開発環境
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
### リモートのREPLに接続する
### セルフホストなClojureScriptのREPLを開始する
### Leiningenプロジェクトの外での利用
### REPLの中で入力する
### REPLにコードを送る
#### ブロックを送る
#### Markdownブロック
#### 選択したコードを送る
### 自動補完
### インラインでの結果表示
### 自動評価モード
### ローカル バインディング値の保存と閲覧
## Proto REPLの拡張
## キーバインドとイベント
