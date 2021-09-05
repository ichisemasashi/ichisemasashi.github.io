---
layout: post
title: Analog時計03
date:   2021-09-04
categories: ClojureScript ブログ
---

# JSの側のアナログ時計プログラムの作りを整理する。

1. キャンバスの作成・初期化
   1. HTMLファイル内のdiv要素を取得
   2. div要素の縦横から短い方を取得
   3. 時計の半径を取得
   4. キャンバスを作成
   5. キャンバスの描画コンテキストを取得
   6. キャンバスの描画サイズを設定
   7. キャンバスの表示サイズを設定
   8. キャンバスをdiv要素の中央に設定
   9. キャンバスにスタイルを設定
   10. 描画の原点をキャンバスの中心に設定
2. 中央の円を描く
   1. 描画のためのデータ(半径A、線の太さB、線の色、塗り潰しの色)
   2. 半径Aの円を書く
   3. 内側の円を書く(半径:A - B)
3. 文字盤を描く
   1. 描画のためのデータ(外周の円の太さ、外周の色、太い目盛の線の太さ、太い目盛の線の長さ、細い目盛の線の太さ、細い目盛の線の長さ、数字の中心の外周からの距離、数字の色、数字のフォント)
   2. パスを記憶する関数
   3. 外側の円を描画
   4. 目盛の表示
      1. 表示位置を外周分内側に置く
      2. 一目盛の角度を取得
      3. 太い線のパスを取得
      4. 細い線のパスを取得
   5. 回転させる前のコンテキストを保存
   6. キャンバスを回転させながら目盛を描く
   7. 保存したコンテキストを復元
   8. 時刻文字を表示
   9. 文字の基準位置・フォントを設定
4. 針(時針、分針、秒針)を描く
   1. 針描画のためのデータ(幅、色、長さ(半径に対する割合)、反対側に飛び出る長さ(半径に対する割合)、一周の分割数)x3
   2. パスの作成
5. 時計を動かす

