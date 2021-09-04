---
layout: post
title: Analog時計00
date:   2021-09-04
categories: ClojureScript ブログ
---

# 環境準備

## clj-newのバージョンチェック

[clj-newのサイト](https://github.com/seancorfield/clj-new)をチェックすると、少しバージョンアップしていましたので、`~/.clojure/deps.edn`を修正。

```
 :aliases {
   :new {:extra-deps {com.github.seancorfield/clj-new
                     {:mvn/version "1.1.331"}}
         :exec-fn clj-new/create}
         :exec-args {}}
```

## プロジェクトの生成

```
% clj -X:new :template figwheel-main :name analog-clock :args '["+deps" "--reagent"]'
Downloading: com/github/seancorfield/clj-new/1.1.331/clj-new-1.1.331.pom from clojars
Downloading: org/clojure/tools.deps.alpha/0.12.1003/tools.deps.alpha-0.12.1003.pom from central
Downloading: org/clojure/pom.contrib/1.1.0/pom.contrib-1.1.0.pom from central
Downloading: org/apache/maven/resolver/maven-resolver-spi/1.7.0/maven-resolver-spi-1.7.0.pom from central
Downloading: org/apache/maven/resolver/maven-resolver-connector-basic/1.7.0/maven-resolver-connector-basic-1.7.0.pom from central
Downloading: org/apache/maven/resolver/maven-resolver-transport-http/1.7.0/maven-resolver-transport-http-1.7.0.pom from central
Downloading: org/apache/maven/maven-resolver-provider/3.8.1/maven-resolver-provider-3.8.1.pom from central
Downloading: org/apache/maven/resolver/maven-resolver-impl/1.7.0/maven-resolver-impl-1.7.0.pom from central
Downloading: org/apache/maven/resolver/maven-resolver-util/1.7.0/maven-resolver-util-1.7.0.pom from central
Downloading: org/apache/maven/resolver/maven-resolver-api/1.7.0/maven-resolver-api-1.7.0.pom from central
Downloading: org/apache/maven/resolver/maven-resolver/1.7.0/maven-resolver-1.7.0.pom from central
Downloading: org/apache/maven/resolver/maven-resolver-transport-file/1.7.0/maven-resolver-transport-file-1.7.0.pom from central
Downloading: org/apache/maven/maven/3.8.1/maven-3.8.1.pom from central
Downloading: org/apache/maven/maven-core/3.8.1/maven-core-3.8.1.pom from central
Downloading: org/clojure/tools.gitlibs/2.3.167/tools.gitlibs-2.3.167.pom from central
Downloading: org/apache/maven/resolver/maven-resolver-named-locks/1.7.0/maven-resolver-named-locks-1.7.0.pom from central
Downloading: javax/annotation/javax.annotation-api/1.3.2/javax.annotation-api-1.3.2.pom from central
Downloading: org/apache/httpcomponents/httpcore/4.4.14/httpcore-4.4.14.pom from central
Downloading: org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.pom from central
Downloading: org/apache/maven/maven-model/3.8.1/maven-model-3.8.1.pom from central
Downloading: org/apache/maven/maven-model-builder/3.8.1/maven-model-builder-3.8.1.pom from central
Downloading: org/apache/maven/maven-repository-metadata/3.8.1/maven-repository-metadata-3.8.1.pom from central
Downloading: org/apache/maven/maven-settings/3.8.1/maven-settings-3.8.1.pom from central
Downloading: org/apache/httpcomponents/httpcomponents-client/4.5.13/httpcomponents-client-4.5.13.pom from central
Downloading: org/apache/httpcomponents/httpcomponents-core/4.4.14/httpcomponents-core-4.4.14.pom from central
Downloading: org/apache/maven/maven-builder-support/3.8.1/maven-builder-support-3.8.1.pom from central
Downloading: org/apache/maven/maven-settings-builder/3.8.1/maven-settings-builder-3.8.1.pom from central
Downloading: org/apache/maven/maven-artifact/3.8.1/maven-artifact-3.8.1.pom from central
Downloading: org/apache/maven/maven-plugin-api/3.8.1/maven-plugin-api-3.8.1.pom from central
Downloading: commons-io/commons-io/2.5/commons-io-2.5.pom from central
Downloading: org/apache/commons/commons-parent/39/commons-parent-39.pom from central
Downloading: org/apache/maven/maven-settings/3.8.1/maven-settings-3.8.1.jar from central
Downloading: org/apache/maven/resolver/maven-resolver-transport-http/1.7.0/maven-resolver-transport-http-1.7.0.jar from central
Downloading: org/apache/maven/maven-model-builder/3.8.1/maven-model-builder-3.8.1.jar from central
Downloading: javax/annotation/javax.annotation-api/1.3.2/javax.annotation-api-1.3.2.jar from central
Downloading: org/apache/httpcomponents/httpcore/4.4.14/httpcore-4.4.14.jar from central
Downloading: org/apache/maven/resolver/maven-resolver-transport-file/1.7.0/maven-resolver-transport-file-1.7.0.jar from central
Downloading: org/apache/maven/maven-core/3.8.1/maven-core-3.8.1.jar from central
Downloading: org/apache/maven/maven-settings-builder/3.8.1/maven-settings-builder-3.8.1.jar from central
Downloading: org/apache/maven/resolver/maven-resolver-api/1.7.0/maven-resolver-api-1.7.0.jar from central
Downloading: org/clojure/tools.deps.alpha/0.12.1003/tools.deps.alpha-0.12.1003.jar from central
Downloading: org/apache/maven/maven-resolver-provider/3.8.1/maven-resolver-provider-3.8.1.jar from central
Downloading: org/apache/maven/resolver/maven-resolver-spi/1.7.0/maven-resolver-spi-1.7.0.jar from central
Downloading: org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.jar from central
Downloading: org/apache/maven/resolver/maven-resolver-impl/1.7.0/maven-resolver-impl-1.7.0.jar from central
Downloading: org/clojure/tools.gitlibs/2.3.167/tools.gitlibs-2.3.167.jar from central
Downloading: org/apache/maven/resolver/maven-resolver-connector-basic/1.7.0/maven-resolver-connector-basic-1.7.0.jar from central
Downloading: org/apache/maven/maven-model/3.8.1/maven-model-3.8.1.jar from central
Downloading: org/apache/maven/resolver/maven-resolver-util/1.7.0/maven-resolver-util-1.7.0.jar from central
Downloading: org/apache/maven/resolver/maven-resolver-named-locks/1.7.0/maven-resolver-named-locks-1.7.0.jar from central
Downloading: org/apache/maven/maven-builder-support/3.8.1/maven-builder-support-3.8.1.jar from central
Downloading: org/apache/maven/maven-repository-metadata/3.8.1/maven-repository-metadata-3.8.1.jar from central
Downloading: org/apache/maven/maven-plugin-api/3.8.1/maven-plugin-api-3.8.1.jar from central
Downloading: org/apache/maven/maven-artifact/3.8.1/maven-artifact-3.8.1.jar from central
Downloading: com/github/seancorfield/clj-new/1.1.331/clj-new-1.1.331.jar from clojars
Downloading: figwheel-main/lein-template/maven-metadata.xml from clojars
Generating fresh figwheel-main project.
  To get started:
  -->  Change into the 'analog-clock' directory
  -->  Start build with 'clojure -A:fig:build'

```

なぜか、my-cljs-projects/analog-clockでなく、analog-clock配下にプロジェクトができていました。

とりあえず、中身を移動します。

```
% tree .
.
├── README.md
├── deps.edn
├── dev.cljs.edn
├── figwheel-main.edn
├── index.html
├── resources
│   └── public
│       ├── css
│       │   └── style.css
│       ├── index.html
│       └── test.html
├── src
│   └── my_cljs_projects
│       └── analog_clock.cljs
├── target
│   └── public
├── test
│   └── my_cljs_projects
│       ├── analog_clock_test.cljs
│       └── test_runner.cljs
└── test.cljs.edn

9 directories, 12 files
```

## デフォルト状態の起動確認

```
% clj -A:fig:build
DEPRECATED: Libs must be qualified, change reagent => reagent/reagent (deps.edn)
WARNING: Use of :main-opts with -A is deprecated. Use -M instead.
2021-09-04 13:44:07.663:INFO::main: Logging initialized @12046ms to org.eclipse.jetty.util.log.StdErrLog
[Figwheel] Validating figwheel-main.edn
[Figwheel] figwheel-main.edn is valid \(ツ)/
[Figwheel] Compiling build dev to "target/public/cljs-out/dev-main.js"
[Figwheel] Successfully compiled build dev to "target/public/cljs-out/dev-main.js" in 23.904 seconds.
[Figwheel] Outputting main file: target/public/cljs-out/dev-main-auto-testing.js
[Figwheel] Watching paths: ("test" "src") to compile build - dev
[Figwheel] Starting Server at http://localhost:9500
[Figwheel] Starting REPL
Prompt will show when REPL connects to evaluation environment (i.e. a REPL hosting webpage)
Figwheel Main Controls:
          (figwheel.main/stop-builds id ...)  ;; stops Figwheel autobuilder for ids
          (figwheel.main/start-builds id ...) ;; starts autobuilder focused on ids
          (figwheel.main/reset)               ;; stops, cleans, reloads config, and starts autobuilder
          (figwheel.main/build-once id ...)   ;; builds source one time
          (figwheel.main/clean id ...)        ;; deletes compiled cljs target files
          (figwheel.main/status)              ;; displays current state of system
Figwheel REPL Controls:
          (figwheel.repl/conns)               ;; displays the current connections
          (figwheel.repl/focus session-name)  ;; choose which session name to focus on
In the cljs.user ns, controls can be called without ns ie. (conns) instead of (figwheel.repl/conns)
    Docs: (doc function-name-here)
    Exit: :cljs/quit
 Results: Stored in vars *1, *2, *3, *e holds last exception object
[Rebel readline] Type :repl/help for online help info
Opening URL http://localhost:9500
ClojureScript 1.10.773
cljs.user=> 
```

無事起動し、ブラウザでページが表示されました。

次回は、ソースコードから不要な行を削除してみたいです。(写経をしたときには、テスト関連で消しきれなかった行があったので...)