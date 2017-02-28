---
layout: post
title: Dockerを始める
date:   2017-02-28
categories: Docker
---
# Dockerを使い始める

[Get started with Docker](https://docs.docker.com/engine/getstarted/)を読んでみる。

※ もちろん、Google翻訳を活用しています。

内容
- お使いのプラットフォーム用のDockerソフトウェアをインストールする
- コンテナ内でソフトウェアイメージを実行する
- Docker Hubで画像をブラウズする
- 自分のイメージを作成し、それをコンテナで実行する
- Docker Hubアカウントとイメージリポジトリを作成する
- 自分のイメージを作成する
- あなたのイメージをDocker Hubにプッシュして他の人が使用できるようにする

# Dockerをインストールしてhello-worldを実行する
1. Dockerの取得

 Mac用、Linux用、Win用がある。
1. Dockerをインストール
1. インストールを確認する。
```
# Dockerの版数を表示し、最新版であることを確認する。
$ docker version
# 実行中のコンテナを表示する。
$ docker ps
$ docker run hello-world
$ docker ps -a
```

# イメージとコンテナについて

イメージとは、実行時に使用するファイルシステムとパラメータです。 それは状態を持っておらず、決して変化しません。 コンテナはイメージの実行インスタンスです。

`docker run hello-world`を実行したときの詳細
1. `hello-world`イメージがあるかどうかを確認する
1. Docker Hubからイメージをダウンロードする
1. イメージをコンテナにロードし、それを実行する

# 「whalesyイメージ」を検索して実行する
- **ステップ1** 「whalesyイメージ」を見つける。
 1. ブラウザから、[Docker Hub](https://hub.docker.com/)を開く ([Strage.Docker](https://store.docker.com/)もあるみたいです。)
 1. 検索バーに「whalesay」という語を入力します。
 1. 検索結果の「 docker/whalesay」をクリックする

- **ステップ2** 「whalesayイメージ」を実行する
 1. コマンドラインターミナルを開きます。
 1. `docker run docker/whalesay cowsay boo`コマンドを入力し、`RETURN`を押します。
```
$ docker run docker/whalesay cowsay boo
```
 1. コマンドラインターミナルで`docker images`コマンドを入力し、`RETURN`キーを押します。
このコマンドは、ローカルシステム上のすべてのイメージを一覧表示します。 一覧ににdocker/whalesayがあることを確認する。
 1. Whalesayのコンテナでちょっと遊んでみてください。
```
$ docker run docker/whalesay cowsay boo-boo
```

# 自分のイメージを構築する
- **ステップ1** 「Dockerfile」を作成する
 1. 新しいディレクトリを作成します。
 1. 新しいディレクトリに移動します。
 1. 現在のディレクトリにあるDockerfileという名前の新しいテキストファイルを編集する。
 LinuxのMacでnanoや`vi`を、Windowsでは`notepad`でします。
 1. 次の行をファイルにコピーして、 `FROM`文を追加します。FROMキーワードは、画像がどの画像に基づいているかをDockerに伝えます。
 ```
 FROM docker/whalesay:latest
 ```
 1. `fortunes`プログラムをイメージにインストールする`RUN`ステートメントを追加します。
 whalesayイメージは、 `apt-get`を使ってパッケージをインストールするUbuntuをベースにしています。 これらの2つのコマンドは、イメージで利用可能なパッケージのリストを更新し、 `fortunes`プログラムをそこにインストールします。 `fortunes`プログラムは、私たちのクジラが賢明な言葉をプリントアウトします。
 ```
 RUN apt-get -y update && apt-get install -y fortunes
 ```
 1. `CMD`文を追加する。
 その環境が設定された後に実行される最終コマンドがイメージに表示されます。 このコマンドは`fortune -a`を実行し、その出力を`cowsay`コマンドに送信します。
 ```
 CMD /usr/games/fortune -a | cowsay
 ```
 1. あなたの仕事を確認してください。 ファイルは次のようになります。
 ```
 FROM docker/whalesay:latest
 RUN apt-get -y update && apt-get install -y fortunes
 CMD /usr/games/fortune -a | cowsay
 ```
 1. ファイルを保存し、テキストエディタを閉じます。 この時点で、ソフトウェアレシピはDockerfileファイルに記述されてDockerfileます。 新しいイメージを作成する準備ができました。

- **ステップ2** Dockerfileからイメージを作成する

 `docker build`コマンドを使用してイメージを作成します。 -tパラメータはイメージにタグを与えますので、後で簡単に実行できます。 `.`を忘れないでください. これは現在のディレクトリでDockerfileというファイルを探すようにdocker buildコマンドに指示します。
```
$ docker build -t docker-whale .
```
- **ステップ3** ビルドプロセスについて学ぶ

このセクションでは、各メッセージの意味を学習します。

1. Dockerはビルドに必要なものがすべてあることを確認します。 これにより、次のメッセージが生成されます。
```
Sending build context to Docker daemon 2.048 kB
```
1. whalesayは、 whalesayイメージがローカルにあるかどうかを確認し、そうでない場合はwhalesayハブから取得します。 この場合、イメージは前のタスクで引っ張ったためにローカルに存在します。 これは、DockerfileのFROM文に対応し、次のメッセージを生成します。
```
Step 1 : FROM docker/whalesay:latest
 ---> 6b362a9f73eb
```
各ステップの最後に、IDが印刷されます。 これは、このステップで作成されたレイヤーのIDです。 Dockerfileの各行は、イメージ内のレイヤーに対応しています。

1. Dockerは、 whalesayイメージを実行するwhalesay起動します（下のRunning in行）。 一時的なコンテナでは、DockerはDockerfileで次のコマンドをRUNます。これはRUNコマンドであり、 fortuneコマンドをインストールします。 Ubuntuホスト上でapt-getコマンドを手動で実行していた場合と同様に、これは多くの出力行を生成します。
```
 Step 2 : RUN apt-get -y update && apt-get install -y fortunes
 ---> Running in 05d4eda04526
Ign http://archive.ubuntu.com trusty InRelease
Get:1 http://archive.ubuntu.com trusty-updates InRelease [65.9 kB]
Get:2 http://archive.ubuntu.com trusty-security InRelease [65.9 kB]
```
....
```
Setting up fortunes (1:1.99.1-7) ...
Processing triggers for libc-bin (2.19-0ubuntu6.6) ...
 ---> dfaf993d4a2e
Removing intermediate container 05d4eda04526
```
RUNコマンドが終了すると、新しいレイヤーが作成され、中間コンテナが削除されます。

1. 新しい中間コンテナが作成され、DockerはDockerfileにCMD行のレイヤーを追加し、中間コンテナを削除します。
```
Step 3 : CMD /usr/games/fortune -a | cowsay
 ---> Running in a8e6faa88df3
 ---> 7d9495d03763
Removing intermediate container a8e6faa88df3
Successfully built 7d9495d03763
```
これで、docker-whaleと呼ばれるイメージが作成できました。


- **ステップ4** 新しい「docker-whale」を実行する
 1. ターミナルウィンドウまたはコマンドプロンプトで、ローカルにあるイメージを一覧表示する`docker images`入力します。
```
$ docker images

REPOSITORY           TAG          IMAGE ID          CREATED             SIZE
docker-whale         latest       c2c3152907b5      4 minutes ago       275.1 MB
docker/whalesay      latest       fb434121fc77      4 hours ago         247 MB
hello-world          latest       91c95931e552      5 weeks ago         910 B
```

 1. `docker run docker-whale`と入力して、新しいイメージを実行します。

```
$ docker run docker-whale

 ______________________________________
< You will be successful in your work. >
 --------------------------------------
    \
     \
      \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/
```

 1. もういちど楽しむために、もう一度実行してください。

# Docker Hubアカウントとリポジトリを作成する
# タグ付け、プッシュ、イメージのプル
