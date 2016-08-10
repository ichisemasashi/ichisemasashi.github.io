---
layout: post
title: howmの設定
date:   2016-08-11
categories: Emacs
---
# 久しぶりにhowmを使いました。

## インストールは簡単

[package-installでイケる](http://howm.osdn.jp/index-j.html)

## ```M-x homw-menu``` or ```C-c , ,```でメニューが出る。
あとは何とかする。

## リンクを貼れる
```>>> ほげ```と書いて, その上で[return]キーを押すと，「ほげ」を含むメモの一覧にジャンプします.

```<<< ほげ```と書くと, 全メモ中の「ほげ」に下線が付きます.

下線が付かないときは, ```C-c , ,```でメニューを開き, [更新] で[return]キー


## 書き出されるファイルをDropboxに入れて管理したい。

Emacsの設定で、howmディレクトリを指定する。


## ```(require)```しないと、動かない。
