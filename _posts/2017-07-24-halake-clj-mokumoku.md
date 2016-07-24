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

## Clojureスタート
```
$ lein repl
Retrieving org/clojure/tools.nrepl/0.2.12/tools.nrepl-0.2.12.pom from central
Retrieving org/clojure/pom.contrib/0.1.2/pom.contrib-0.1.2.pom from central
Retrieving org/sonatype/oss/oss-parent/7/oss-parent-7.pom from central
Retrieving clojure-complete/clojure-complete/0.2.4/clojure-complete-0.2.4.pom from clojars
Retrieving org/clojure/clojure/1.8.0/clojure-1.8.0.pom from central
Retrieving org/clojure/tools.nrepl/0.2.12/tools.nrepl-0.2.12.jar from central
Retrieving org/clojure/clojure/1.8.0/clojure-1.8.0.jar from central
Retrieving clojure-complete/clojure-complete/0.2.4/clojure-complete-0.2.4.jar from clojars
nREPL server started on port 34360 on host 127.0.0.1 - nrepl://127.0.0.1:34360
REPL-y 0.3.7, nREPL 0.2.12
Clojure 1.8.0
OpenJDK 64-Bit Server VM 1.8.0_91-8u91-b14-0ubuntu4~14.04-b14
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
    Exit: Control+D or (exit) or (quit)
 Results: Stored in vars *1, *2, *3, an exception in *e

user=> (pr "Hello world!")
"Hello world!"nil
user=> (doc doc)
-------------------------
clojure.repl/doc
([name])
Macro
  Prints documentation for a var or special form given its name
nil
user=>
```

## Emacsのインストール

```
$ sudo apt-get install emacs24-nox emacs24-el
$ sudo emacs -version
GNU Emacs 24.3.1
Copyright (C) 2013 Free Software Foundation, Inc.
GNU Emacs comes with ABSOLUTELY NO WARRANTY.
You may redistribute copies of Emacs
under the terms of the GNU General Public License.
For more information about these matters, see the file named COPYING.
```

## Emacs上でのClojure開発のためのパッケージ導入

- clojure mode
- cider
- paredit
- rainbow delimiters
- ddskk

## 発表したこと。
quilやgenerative designについて、以下のURLを紹介しながら喋りました。
http://bugrammer.hateblo.jp/entry/2015/08/06/092010
https://github.com/quil/quil
https://medium.com/@thi.ng/workshop-report-generative-design-with-clojure-7d6d8ea9a6e8#.of9da7w48
https://github.com/quil/quil-examples/tree/master/src/quil_sketches/gen_art

http://quil.info/examples
https://github.com/overtone/emacs-live
