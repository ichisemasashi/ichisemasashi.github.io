---
layout: post
title: cljs-ajax
date:   2021-11-14
categories: 勝手翻訳 ClojureScript
---

# cljs-ajax

ClojureScriptとClojureのためのシンプルなAjaxクライアント。

[![CircleCI](https://circleci.com/gh/JulianBirch/cljs-ajax.svg?style=shield)](https://circleci.com/gh/JulianBirch/cljs-ajax)

`cljs-ajax` は Clojure と ClojureScript の両方で (有用であれば) 同じインタフェースを公開します。ClojureScript上では、[`goog.net.XhrIo`](https://developers.google.com/closure/library/docs/xhrio?hl=en)や[`js/XmlHttpRequest`](https://developer.mozilla.org/ja/docs/Web/API/XMLHttpRequest)のラッパーとして動作し、JVM上では、[Apache HttpAsyncClient](https://hc.apache.org/httpcomponents-asyncclient-4.1.x/index.html)ライブラリのラッパーとして動作します。 

このドキュメントに加えて、[FAQ](docs/faq.md)、[change log](CHANGES.md)、[contribution document](CONTRIBUTING.md)があります。さらに、[docsフォルダ](docs)には、特定の機能や設計上のアドバイスに関する詳細なドキュメントがあります。

## Usage

Leiningen/Boot: `[cljs-ajax "0.7.5"]`

[![Leiningen version](http://clojars.org/cljs-ajax/latest-version.svg)](http://clojars.org/cljs-ajax)

クライアントは `GET`, `POST`, `PUT` 関数を使ってサーバにAjaxリクエストを送信する簡単な方法を提供します。また、`ajax-request`を使った簡単な方法も提供しています。すべてのリクエストは非同期で、レスポンスやエラー処理のためのコールバック関数を受け付けます。

現在、サーバーとの通信には4つのフォーマットがサポートされています。 `:transit`, `:json`, `:text`, `:raw` です。
(`:text` は、通常のフォーム送信でパラメータを送信し、生のテキストを返します。`:raw` は同じことをしますが、JVM上ではボディの `java.io.InputStream` を返し、*それを閉じません*)。

Clojure で `cljs-ajax` を使って動作するようにサーバー側を設定する方法については、[handling responses on the server](docs/server.md) のページを参照してください。

## GET/POST/PUT

`GET`、`POST`、`PUT`の各ヘルパーは、URIに続いてオプションのマップを受け取ります。

* `:handler` - 操作が成功したときのハンドラ関数は、デシリアライズされたレスポンスを1つのパラメータとして受け取ります。ハンドラを提供しない場合には、代わりに `default-handler` アトムのコンテンツが呼び出されます。デフォルトでは、これは `println` です。
* `:progress-handler` - プログレスイベントのハンドラ関数です。**このハンドラは、`goog.net.XhrIo` APIを使用している場合にのみ利用できます**。
* `:error-handler` - エラー用のハンドラー関数で、エラーレスポンス(以下に詳述)を受け取る必要があります。エラーハンドラを提供しない場合は、代わりに `default-error-handler` アトムの内容が呼び出されます。デフォルトでは、Clojureでは `println` となり、ClojureScriptではコンソールにエラーを書き込みます。
* `:finally` - パラメータを取らない関数で、他のハンドラに加えて、コールバック中に起動されます。
* `:format` - リクエストのボディのフォーマットを指定します (Transit、JSONなど)。また、適切な `Content-Type` ヘッダーを設定します。 指定されていない場合は、デフォルトで `:transit` となります。
* `:response-format` - サーバーから特定のフォーマットのデータを受け取りたいことを指定します (`Accept` ヘッダーを設定して、レスポンスを希望のフォーマットとして解析するように強制します)。 提供されない場合は、寛容な `Accept` ヘッダーが送信され、レスポンスのボディはレスポンスの `Content-Type` ヘッダーに従って解釈されます。
* `:params` - リクエストと一緒に送信されるパラメータです。フォーマットに依存します。`:transit` と `:edn` は何でも送信できますが、`:json`, `:text`, `:raw` はマップを指定する必要があります。 `GET` はクエリ文字列にパラメータを追加し、`POST` はボディにパラメータを追加します。
* `:url-params` - クエリ文字列に追加されるパラメータです。`GETリクエストの場合、ここで定義されたパラメータは、`:params`で定義されたパラメータを置き換えます。
* `:timeout` - ajax コールのタイムアウトをミリ秒単位で指定します。 デフォルトは `0` (タイムアウトなし)です。
* `:headers` - リクエストで設定する HTTP ヘッダーのマップです。
* `:cookie-policy` - クッキー管理の仕様を表すキーワードです。**Javaでのみ利用可能です**。オプションです。`:none`, `:default`, `:netscape`, `:standard`, `:standard-strict` のいずれかです。

* `:with-credentials` - XHR オブジェクトに `withCredentials` フラグを設定するかどうかを示すブール値です。
* `:body` - リクエストで送信する正確なデータです。指定された場合、`:params` と `:request-format` の両方は無視されます。 js/FormData やその他の "raw" javascript タイプを、これを通して送信することができることに注意してください。
* `:interceptors` - このリクエストに対して実行する [interceptors](docs/interceptors.md) です。セットされていない場合は、`default-interceptors`グローバルアトムのコンテンツを実行します。これはデフォルトでは空のベクトルです。詳細については、[interceptors page](docs/interceptors.md)をご覧ください。

`default-handler`と`default-error-handler`をオーバーライドすることができますが、これらはアプリケーション/ページに対してグローバルなものであることに注意してください。

### JSON specific settings

以下の設定は、JSONレスポンスの解釈に影響を与えます。 (これらを使用するには、`:response-format` に `:json` を指定する必要があります。)

* `:keywords?` - マップ内のキーがキーワード化されるかどうかを true/false で指定します。
* `:prefix` - JSONレスポンスの先頭から取り除かれるプレフィックスです。例えば、`while(1);`は、[JSONハイジャックを防ぐ]ためにいくつかのAPIで追加されています(https://stackoverflow.com/questions/2669690/why-does-google-prepend-while1-to-their-json-responses)。 GETリクエストの場合は、*必ず*これを使うべきです。

### GET specific settings

`:vec-strategy`の設定は、シーケンスがどのように書き出されるかに影響を与えます。
`:java` の `:vec-strategy` は、`{:a [1 2]}` を `a=1&a=2` としてレンダリングします。
`:rails` の `:vec-strategy` は `{:a [1 2]}` を `a[]=1&a[]=2` としてレンダリングします。これは、HTTPで動作させる場合にも正しい設定です。

### GET/POST examples

```clojure
(ns foo
  (:require [ajax.core :refer [GET POST]]))

(defn handler [response]
  (.log js/console (str response)))

(defn error-handler [{:keys [status status-text]}]
  (.log js/console (str "something bad happened: " status " " status-text)))

(GET "/hello")

(GET "/hello" {:params {:foo "foo"}})

(GET "/hello" {:params {:a 0
                        :b [1 2]
                        :c {:d 3 :e 4}
                        "f" 5}})
;;; writes "a=0&b=1&b=2&c[d]=3&c[e]=4&f=5"

(GET "/hello" {:params {:a 0
                        :b [1 2]
                        :c {:d 3 :e 4}
                        "f" 5}
               :vec-strategy :rails})
;;; writes "a=0&b[]=1&b[]=2&c[d]=3&c[e]=4&f=5"


(GET "/hello" {:handler handler
               :error-handler error-handler})

(POST "/hello")

; Post a transit format message
(POST "/send-message"
        {:params {:message "Hello World"
                  :user    "Bob"}
         :handler handler
         :error-handler error-handler})


; フォームに入力されたファイルを送信します。
(POST "/send-form-modern" {:body (js/FormData. form-element)})

; Send file explicitly, ClojureScript specific
(let [form-data (doto
                    (js/FormData.)
                  (.append "id" "10")
                  (.append "file" js-file-value "filename.txt"))]
  (POST "/send-file" {:body form-data
                      :response-format (raw-response-format)
                      :timeout 100}))

; 複数のファイルを明示的に送る、ClojureScript固有の機能
; input-elementは、ファイルタイプのhtml要素です。
(let [form-data (let [f-d (js/FormData.)
      files (.-files input-element)]
     (doseq [file-key (.keys js/Object files)]
              (.append f-d file-key            
                   (aget files file-key)))
                    f-d)]
    (POST "/send-files" {:body form-data
                         :response-format (raw-response-format)
                         :timeout 1000}))
                      
(POST "/send-message"
        {:params {:message "Hello World"
                  :user    "Bob"}
         :handler handler
         :error-handler error-handler
         :response-format :json
         :keywords? true})
         
(PUT "/add-item"
     {:params {:id 1 :name "mystery item"}})
     
(GET {:url "/generate.png" ; Request a PNG and get it back as a js/ArrayBuffer
      :response-format {:content-type "image/png" :description "PNG image" :read -body :type :arraybuffer}})
```

### FormData support

なお、`js/FormData` は IE10 以前はサポートされていませんので、それらのブラウザをサポートする必要がある場合は使用しないでください。 `cljs-ajax` には、ファイルアップロードに関するその他のサポートはありません (ただし、プルリクエストは歓迎します)。 また、`js/FormData` は、ファイルの送信に使わなくても、常にマルチパートとして送信されるので、リングハンドラには `ring.middleware.multipart-params/wrap-multipart-params` を含める必要があることに注意してください。

### Error Responses

エラーレスポンスは、以下のキーが渡されたマップです。

* `:status` - HTTPステータスコード（数値）です。サーバーにたどり着けなかった場合には、ダミーの数字が提供されます。
* `:status-text` - HTTPステータスメッセージ、または解析失敗時のフィードバックです。
* `:failure` - 失敗の種類を表すキーワードです。
  * `:error` - サーバーで発生したエラーです。
  * `:parse` サーバーからのレスポンスの解析に失敗しました。
  * `:aborted` クライアントがリクエストを中止した。
  * `:timeout` リクエストがタイムアウトしました。

失敗したときに有効なレスポンスがあった場合には、そのレスポンスが `:response` キーに格納されます。

エラーが `:parse` であった場合には、レスポンスの生のテキストが `:original-text` に格納されます。

最後に、サーバーがエラーを返し、その結果として解析に失敗した場合には、エラーマップを返しますが、解析の失敗を含む `:parse-error` というキーを追加します。

`GET`, `POST`, `PUT` の `error-handler` には、1つのパラメータが渡され、そのパラメータにはエラーレスポンスが含まれます。 *どのような場合でも*`handler`または`error-handler`のどちらかが呼ばれることに注意してください。 `GET`や`POST`などで返される例外は、絶対に取得してはいけません。

## ajax-request

`ajax-request`はシンプルなインターフェースです。 GETやPOSTのAPIとは以下のような違いがあります。

* **キーワードでフォーマットを指定することはできません。リクエストには完全なフォーマットを指定する必要があります（詳細は [formats documentation](docs/formats.md) を参照してください）**。
* APIが使いにくくなります。
* 独自のフォーマットを使用することができます。
* 高度な最適化機能をオンにすると、より小さなコードにコンパイルされます。
* Content-Typeの検出ができません。
* ハンドラーが1つしかないので、エラー処理をしなければならない。

パラメータは1つで、以下のメンバーを持つマップとなります。
パラメータは 
* `:uri`
* `:method` - (`:get`, `"GET"`, `:post`, `"POST"` など)  
* `:format` と `:response-format` - [formats documentation](docs/formats.md) に記載されています。
* `:handler` - 単一の引数 `[ok result]` を取る関数です。 結果は、trueの場合はレスポンス、falseの場合はエラーレスポンスになります。

以下のパラメータは、`GET`/`POST`のイージーアピと同じです。
* `:params` - リクエストと一緒に送信されるパラメータで、フォーマットに依存します: `:transit` と `:edn` は何でも送信できますが、 `:json` と `:raw` はマップを指定する必要があります。 `GET` はクエリ文字列にパラメータを追加し、`POST` はボディにパラメータを追加します。
* `:timeout` - ajaxコールのタイムアウトです。 空白の場合は30秒です。
* `:headers` - リクエストで設定される HTTP ヘッダーのマップです。
* `:cookie-policy` - クッキー管理の仕様を表すキーワードです。**Javaでのみ利用可能です**。オプションです。`:none`, `:default`, `:netscape`, `:standard`, `:standard-strict` のいずれかです。
* `:with-credentials` - XHRオブジェクトに `withCredentials` フラグを設定するかどうかを示すブール値です。
* `:interceptors` - このリクエストに対して実行する [interceptors](docs/interceptors.md) です。設定されていない場合は、`default-interceptors`グローバルアトムの内容を実行します。これはデフォルトでは空のベクトルです。詳細については、[interceptors page](docs/interceptors.md)をご覧ください。

### `ajax-request` examples

```clj
(defn handler2 [[ok response]]
  (if ok
    (.log js/console (str response))
    (.error js/console (str response))))

(ajax-request
        {:uri "/send-message"
         :method :post
         :params {:message "Hello World"
                  :user    "Bob"}
         :handler handler2
         :format (json-request-format)
         :response-format (json-response-format {:keywords? true})})

(ajax-request
        {:uri "/send-message"
         :method :post
         :params {:message "Hello World"
                  :user    "Bob"}
         :handler handler2
         :format (url-request-format) 
         :response-format (json-response-format {:keywords? true})})
```

これらの例では、Google Closureライブラリの`XhrIo` APIを使用します。`XMLHttpRequest` APIを直接使用したい場合は、`:api (js/XMLHttpRequest.)`をマップに追加してください。


