---
layout: post
title: Laketown.clj #4 - Clojureもくもく会
date:   2016-07-24
categories: Clojure
---
# [Laketown.clj #4 - Clojureもくもく会](https://halake.doorkeeper.jp/events/47726)に参加しました。

## 開発環境の再整備

[Cloud9](https://c9.io/)で古い環境がディスクFullになったので、新しい環境を用意します。
- 「blank」指定でWorkSpaceを作成。
- システムの確認


```
$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04.3 LTS"
$ df -H .
Filesystem      Size  Used Avail Use% Mounted on
none            2.3G  4.2M  2.2G   1% /
```

- コンソールのプロンプトを変更

```
<ubuntu@ichisemasashi-clj-2016-07-3558530:~>
$
```

- openJDK 8をインストール

```
$ sudo add-apt-repository ppa:openjdk-r/ppa
$ sudo apt-get update
$ sudo apt-get install openjdk-8-jdk
```

- Leiningen のインストール

```
$ mkdir ~/bin
$ cd ~/bin
$ curl -L -O https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein
$ chmod +x lein
$ lein
```
from [tnoda-clojure](http://tnoda-clojure.tumblr.com/post/111489802935/getting-started-with-clojure-1-installing-clojure-and-le)

> Download the lein script (or on Windows lein.bat)<br>
> Place it on your $PATH where your shell can find it (eg. ~/bin)<br>
> Set it to be executable (chmod a+x ~/bin/lein)<br>
> Run it (lein) and it will download the self-install package

from [Leiningen](http://leiningen.org/)

- Javaのバージョン問題
openJDK 8をインストールしたのに、lein -vで表示されるJavaのバージョンとjavac -vで表示されるJavaのバージョンが違う。
> マツバさんに助けてもらって、システムにインストールされている複数のJava内でのデフォルトを設定変更する。
```
$ update-alternatives --display java
$ java -version
openjdk version "1.8.0_91"
OpenJDK Runtime Environment (build 1.8.0_91-8u91-b14-0ubuntu4~14.04-b14)
OpenJDK 64-Bit Server VM (build 25.91-b14, mixed mode)
$ lein -v
Leiningen 2.6.1 on Java 1.8.0_91 OpenJDK 64-Bit Server VM
```
