---
layout: post
title: SpacemacsにGauche(SICP)を設定してみる
date:   2021-06-30
categories: Clojure 勝手翻訳
---

# SpacemacsにGauche(SICP)を設定してみる

**前提**
- Spacemacsはインストール済み

## GaucheとGauche-compat-sicpのインストール
```bash
$ brew install gauche
```

```bash
$ git clone https://github.com/shirok/Gauche-compat-sicp.git
$ cd Gauche-compat-sicp
$ ./configure
$ make install
```

## GaucheとSpacemacsの設定
[参考](https://practical-scheme.net/gauche/man/gauche-refj/intarakuteibunaKai-Fa-.html)

~/.spacemacs
```elisp
;; for Gauche
(setq scheme-program-name "gosh -i")
```
↑ これで、spacemacs上で、`M-x run-scheme` とすれば、goshが起動する。

~/.gaucherc
```scheme
; https://github.com/shirok/Gauche-compat-sicp
(use compat.sicp)
```
↑ これで、sicp環境がgoshへロードされる。

