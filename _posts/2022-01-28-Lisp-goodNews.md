---
layout: post
title: Lisp: Good News, Bad News, How to Win Big
date:   2022-1-28
categories: Blog 勝手翻訳 Lisp
---

# Lisp 良い知らせ、悪い知らせ、大勝利する方法

Richard P. Gabriel Lucid, Inc

## 概要

Lispはこの10年間、ほぼ標準化され、商業部門の基礎を形成し、優れた性能を達成し、優れた環境を持ち、アプリケーションを提供できるなど、非常に順調であった。しかし、Lispコミュニティはそのような成果を上げることができなかった。この論文では、成功例、失敗例、そして次に何をすべきかを考察します。

Lispの世界は素晴らしい状態にあります。10年前には標準的なLispは存在せず、最も標準的なLispはInterLispで、PDP-10とXerox Lispマシン（Vaxesで動作したという人もいましたが、私はそれは誇張だと思います）、次に標準的なLispはMacLispで、PDP-10のマシンでのみ、そのマシンで最も人気のある3つのOSで動作していました。第4位はZetalispで、2種類のLispマシンで動作しました。第5位はSchemeで、数種類のマシンで動作しましたが、これを使おうとする人はほとんどいませんでした。いずれも今日の基準からすると、性能は低いかぎりぎりで、環境は存在しないかぎりぎりで、他の言語やソフトウェアとの統合は存在しないか不十分で、移植性は低く、受け入れられにくく、商業的な見込みも低いものでした。

現在ではCommon Lisp (CL)があり、主要なマシン、主要なオペレーティングシステム、そして事実上すべての国で稼働している。Common LispはANSIによって標準化されようとしており、性能も良く、良い環境に囲まれており、他の言語やソフトウェアとの統合も良好である。

しかし、ビジネスとしてのLispは不健康だと言われています。実用的なアプリケーションを提供する手段としてのLispは見捨てられたという噂が根強く、時には真実味を帯びています。

この問題は、一般に信じられているよりも優れたLisp配信ソリューションが存在するという認識の問題であり、また、リソースの未配置や誤配備、プロジェクトの未実施、実装戦略の未活性化という問題の一部でもあります。

この問題の一部は、人工知能（AI）ビジネスにおける我々の親愛なる友人から生じています。AIは、人間の知識や問題解決行動を形式化するための優れたアプローチを数多く持っています。しかし、AIは、その適用領域において万能ではありません。初期のAI推進者の中には、商業界への期待値を上げすぎた人もいました。これらの期待は、エキスパート・システム・ベースのアプリケーションの有効性と実現可能性に関係するものだった。

この期待に応えられないと、スケープゴートを探したのですが、それがLispの会社であることが多く、特に実現性という点では、そのような会社が多かったのです。もちろん、もしAI企業が、最終的に市場がAIソフトウェアに何を期待するかについて何らかの考えを持っていたとしても、私が知る限り、Lisp企業とは共有されませんでした。Lisp社は生き残るために必要なことをするのだから、顧客リストや情報を共有する必要はない、というのがAI社の姿勢だったと思います。

もうひとつの問題は、Lispが比較的悪く報道されたことです。Forbes（1989年10月16日号）のJulie PittaのWhere Lisp Slippedというタイトルの記事を見たことがあります。しかし、その記事はSymbolicsとその運命に関するものでした。記事の中でSymbolics社に対する最大の批判は、Symbolics社がAIが普及すると信じていたこと、そしてSymbolics社がAIにはプロプライエタリなハードウェアが適しているという見解を誤って押し通したことです。Lispについては、人工知能の分野で広く使われているやや無名のプログラミング言語であるという記述以外、記事には何も書かれていない。

Lispという名前からかわいいタイトルをつけようとしたJulieのせいで、Lispのビジネスに一石を投じることになったのは残念なことです。

しかし、Lispの成功例もあれば、問題点もあり、それを解決する方法もある。


## 1.0 Lispの成功

先に述べたように、Lispは現在、かつてないほど良い状態にある。ここで、Lispの成功例をいくつか振り返ってみたいと思います。

## 1.1 標準化

大きな成功は、Common Lispという標準的なLispが存在することである。もっとシンプルで、小さくて、きれいなLispがあればと思う人は多いでしょうが、現在あるLispで標準化が可能なのはCommon Lispです。しかし、より良いLispが標準化されないとは言いませんし、そうすべきです。また、Common Lispは他の言語と同様、ニーズの変化に応じて改良・変更されるべきものです。

Common Lispは1981年にSRIで開かれたARPA主催のLispの将来を決める会議の後に、草の根的な活動として始まりました。当時、米国では元MITの人たちが定義・実装しているLispがいくつもありました。Greenblatt (LMI), Moon and Weinreb (Symbolics), Fahlman and Steele (CMU), White (MIT), and Gabriel and Steele (LLNL)であった。Common Lisp委員会の中核はこのグループから生まれた。そのコアとはFahlman、Gabriel、Moon、Steele、Weinrebで、Common Lispはこの人たちが大事にしたLispが合体したものである。

他にもCommon Lispに融合しうるLispsはありましたが、それらはMacLispの伝統を明確に受け継いでおらず、また、それらの支持者は、草の根の努力で定義されたCommon Lispよりも自分たちの方言が成功すると予測し、積極的に参加することを断念しました。このようなLispには、Scheme、Interlisp、Franz Lisp、Portable Standard Lisp、Lisp370などがある。

また、米国以外でもCambridge LispやLe-Lispなどの大規模なLispの取り組みがあった。米国の草の根的な活動は、米国外からの参加者を求めなかったのですが、これは間違いだったと考えてよいでしょう。なぜなら、Common Lispグループの中には、標準Lispの必要性を北米以外に広げるようなAIの将来を見据えた人はほとんどいなかったからです。

Common Lispが定義され、1984年にCommon Lisp: the Language (CLtL)という本が出版された。また、Lispマシンメーカーに対抗してCommon Lispを純正ハードウェアに搭載する会社がいくつか誕生した。4年後には、主要なコンピュータ会社は、自社で実装したCommon Lispか、Common Lispの会社からプライベートラベルを受けたCommon Lispを持つようになった。

1986年、ANSI版のCommon Lispを作成するためにX3J13が設立された。それまでに、Common Lispの曖昧さや抜けをなくし、条件システムを追加し、オブジェクト指向の拡張を定義するために、大きな変更が必要であることが明らかにされていた。

数年後、成熟した言語と優れた定義があっても、標準化のプロセスは単純でないことが明らかになった。Common Lisp Object System (CLOS)の仕様策定だけでも2年近くかかり、X3J13の最も優秀なメンバー7人が参加した。

また、Lispの国際標準化に対する関心が高まっていることも明らかになった。しかし、Common Lispの後継者はいなかった。Common Lispの批評家、特に米国外の批評家は、Common Lispが実用的な提供手段としては失敗していることに着目していた。

1988年、Lispの標準化のための国際的なワーキンググループが結成された。そのグループはWG16と呼ばれている。近い将来の標準LispはCommon Lispであること、Common Lispを超える長期的な標準が望まれること、の2点が絶対的に明確であった。

1988年には、SchemeのIEEE標準、場合によってはANSI標準を作成するために、IEEE Scheme作業部会が結成されました。このグループは1990年に作業を完了し、比較的小さくてきれいなSchemeが標準となりました。

現在、X3J13はANSI Common Lispの標準化ドラフトまであと1年弱、WG16は国際的ないざこざで停滞、SchemeはIEEEで標準化されましたが、商業的な関心は低いようです。

Common Lispは国際的に使われており、常に論争の絶えないLispコミュニティが協力し合うまで、少なくとも事実上の標準として機能しています。


## 1.2 良好なパフォーマンス

Common Lispは性能が良い。現在の実装の多くは最新のコンパイラ技術を利用しています。これに対し、古いLispは当時としては非常に原始的なコンパイラ技術を使っています。性能面では、現在のCommon Lispはほとんどのコンピュータで、PDP-10や1980年代中頃のシングルユーザマシンよりも高い性能が期待できます。Common Lispの実装の多くは、マルチタスクと非侵入型ガベージコレクションを備えていますが、いずれも10年前のハードウェアでは不可能と考えられていた機能です。

以下の表は、3つのベンチマークにおけるLispの実行時間とコードサイズのCの実行時間とコードサイズに対する比を示しています。

| Benchmark | CPU Time | Code Size | 
|:-|:-|:--|
|Tak| 0.90 | 1.21 |
|Traverse| 0.98 | 1.35 |
|Lexer | 1.07 | 1.48 |

`Tak` はガブリエルベンチマークで、関数呼び出しと固定長演算を測定する。`Traverse` はガブリエルのベンチマークで、構造体の生成とアクセスを計測する。`Lexer` は C コンパイラのトークナイザであり、ディスパッチと文字操作を測定する。

これらのベンチマークは 1987 年に Sun 3 上で標準的な Sun C コンパイラを用いて完全最適化で実行されたものです。Lispは非侵入型のガベージコレクタを動作させていない。


## 1.3 良い環境

現代のプログラミング環境は、LispとAIの伝統に由来していることは間違いない。最初のビットマップ端末（Stanford/MIT）、マウスポインティングデバイス（SRI）、フルスクリーンテキストエディタ（Stanford/MIT）、ウィンドウ環境（Xerox PARC）はすべてAI研究に従事していた研究室から生まれたものである。現在でも、プログラミング環境「Symbolics」が最先端であると言うことができる。

また、以下のような開発環境の特徴は、Lispの世界から生まれたと言える。

- インクリメンタルコンパイルとロード 
- シンボリックデバッガ
- データインスペクタ
- ソースコードレベルでのシングルステップ
- 組込み演算子に関するヘルプ 
- ウィンドウベースデバッギング 
- シンボリックスタックバックトレース
- 構造体エディタ


現在のLisp環境は、1970年代のLispマシン環境の最高峰に匹敵するものです。ウィンドウウィングや派手な編集、優れたデバッギングが当たり前になっている。また、ソース管理機能、自動相互参照機能、自動テスト機能など、ソフトウェアのライフサイクルに配慮したLispシステムもあります。

## 1.4 優れた統合性

現在、LispのコードはC、Pascal、Fortran等と共存することができます。これらの言語はLispから呼び出すことができ、一般に、これらの言語はLispを再呼び出すことができます。このようなインタフェースにより、プログラマはLispのデータを外部コードに渡したり、外部データをLispコードに渡したり、Lispコードから外部データを操作したり、外部コードからLispデータを操作したり、外部プログラムを動的にロードしたり、外部関数とLisp関数を自由に混在させたりすることができるようになります。

この機能のための設備は非常に充実しており、複数の異なる言語を一度に混在させる手段を提供する。

## 1.5 オブジェクト指向プログラミング

Lispは、あらゆる言語の中で最も強力で、包括的、かつ広範なオブジェクト指向の拡張機能を備えています。CLOSは、他のオブジェクト指向言語にはない機能を備えている。これには以下のようなものがある。

- 多重継承
- マルチメソッドを含む汎用関数 - 第一級クラス
- ファーストクラスジェネリック関数
- メタクラス
- メソッドの組み合わせ
- 初期化プロトコル
- メタオブジェクトプロトコル
- Lisp型との統合


オブジェクト指向プログラミング言語として標準化されたのは、（CLOSを含む）Common Lispが最初と思われる。

## 1.6 配送について


Lispで書かれたアプリケーションを配信することは可能です。これから述べるように、現在利用可能なツールは良いものですが、まだ理想的なものではありません。本節では、Lucidで成功した配信方法について説明する。

Lucidデリバリーツールキットは3つのツールセットで構成されています。パフォーマンス・モニタリング、リオーガナイゼーション、そしてツリーシェーキングです。

### 1.6.1 パフォーマンス監視ツール


パフォーマンス・モニタリング・ツールは、プログラムの動的な動作に関する情報を提供します。ストレージの割り当てと解放は、しばしばプログラム性能の重要な要素であるため、ツールはストレージの割り当てに関する情報も提供します。これらのツールには目新しいものはありませんが、これらのツールの使用は、プログラム性能を良好にするために、ほとんどの場合、非常に重要です。

### 1.6.2 リオーガナイザー

ワーキング・セットが利用可能な実メモリを超えるプログラムでは、仮想メモリ・システム・コードに費やす時間がユーザ・コードに費やす時間を大きく上回る可能性があります。例えば、作業セット全体でランダムにメモリを参照する命令しか実行しないプログラムがあるとします。プログラムのワーキング・セットを`WS`、そのプログラムが利用できる実メモリを`Mem`、実際のユーザ・コードに費やした時間を`Utime`、プログラムの総実行時間を`Total`とすると、簡単な解析によりTotalは以下の式で表されます。

```
Total = Utime, for WS ≤ Mem
Total = Utime+Pft⋅(WS–Mem)⁄(WS),for WS>Mem
```

`Pft`はページフォルト時間、つまり、ページフォルトを処理するのにかかる時間のことです。一般的なマシンでは、この数値は通常、ページ障害を起こさないメモリ参照命令の実行にかかる時間の1,000倍から10,000倍である。(*1)

Lispプログラムでは、作業セットが使用可能なメモリを超えることがよくあるため、作業セットを減らすためのツールは非常に大きな性能向上につながる可能性がある。作業セットは、動的に割り当てられるオブジェクトへの依存を減らすこと、つまりプログラムのCONSingを減らすことで削減できる場合がある。しかし、作業セットのサイズが主に永続的なオブジェクトへの参照に起因する場合は、他のテクニックが必要になります。

Lucid Delivery Tool Kitでは、Lispアドレス空間内のパーマネントオブジェクトを再編成し、類似の参照パターンを持つオブジェクトを近傍のアドレスに配置するアプローチをとっている。(特に、プログラムから参照されるオブジェクトとそうでないオブジェクトを少なくとも分離することを意味する)。

### 1.6.3 ツリーシェイカー

Lucidを含む多くのLisp開発システムでは、Lispシステムの全ての資源がデフォルトで提供されているため、プログラマは都合の良いツールを使って開発することになります(*2)。 Lisp基本システム（あるいはLisp基本システムの上に構築された開発システム）の多くは、一般にアプリケーションによって使用されないため、これらの未使用部分を取り除くためのツールがあると非常に役に立ちます。このツールをTreeshakerと呼ぶ。(*3)

Treeshakerの実行は、ウォーキング、テスト、ライティングの3つのフェーズで行われます。ウォーキングフェーズでは、Treeshakerは保存画像に含める必要のあるオブジェクトのセットを蓄積する。このセットを作成した後、ツリーシェイカーはアプリケーションのテストを実行し、典型的な実行で使用されるすべてのオブジェクトが含まれていることを確認する。その後、書き込みフェーズでは、アプリケーションを実行するための実行可能なイメージを生成する。

ウォークフェーズは、アプリケーションのトップレベル関数が生成する Lispイメージの連結成分(有向グラフとして扱われる)を計算することで、一応の解決を見ることができる。しかし、Lispオブジェクトは一般に連結されるため、通常、未使用のサブシステムを含むLispイメージのほぼ全体が含まれる。そこでツリーシェイカーは、実際に歩く必要のないオブジェクト間の 接続を見つけるために、いくつかのテクニックを用います。


### 1.6.4 成果

最初の例は、磁気共鳴イメージング装置の位置合わせを支援する簡単なエキスパートシステムである。結果は、オリジナルのプログラムと、再編成されトレースされたバージョンで、短いテストと長いテストの2種類のテストを実行したものである。WSはメガバイト単位の作業セットである。時間は時間：分で表される。

MRI Alignment Program
|  |Short|Long|Size|WS|
|:-|:---|:----|:---|:--|
|Original|2:22|\*:\*\*|4.45mb|3.50mb|
|Final|0:42|4:11|1.65mb|2.25mb|

2つ目の例は、記号代数システムであるReduceである。3つのプログラムは、オリジナル、トレシャク版、トレシャクと再編成版です。CPUの列は、CPUがユーザーコードの実行に費やした時間（秒）で、それ以外の時間はすべてページフォルト処理時間です。

Reduce Computer Algebra System
|   |Size|Time|Faults|CPU|
|:--|:---|:---|:-----|:--|
|Original|6.68mb|6:20|1230|55.2|
|Shaken|2.78mb|3:25|680|52.1|
|Reorganized|2.78mb|1:50|220|50.1|

## 2.0 Lispの見かけ倒し

涙の数が多すぎて、一つの心では泣けない。

一つの心を貫くには、あまりにも多くの涙のしずくがある。

君が僕を捨ててから、君はもうずっと上の空だ。

いつも笑っている、私を見下すように。

**? & The Mysterians**

この幸せな物語には、悲しい幕間がある。その幕間とは、AIが飛躍できなかったことに起因するかもしれないが、おそらく私たちが耳を傾けなければならない別の真実の粒があるのだろう。今日のLispの重要な問題は、2つの相反するソフトウェア哲学の間の緊張関係から生じています。その2つの哲学とは、「正しいこと」と「悪いことは良いこと」と呼ばれるものです。

## 2.1 ワース・イズ・ベターの台頭

私やCommon LispやCLOSの設計者は、MIT/Stanford流のデザインに極端に触れてきたと思います。このスタイルのエッセンスは、**the right thing**というフレーズで捉えることができます。このような設計者にとって重要なことは、以下の特徴をすべて正しく把握することである。

シンプルであること-デザインは実装もインターフェースもシンプルでなければならない。インターフェイスがシンプルであることは、実装よりも重要である。
- 正しさ-設計は観察可能なすべての側面において正しくなければならない。不正確な設計は許されません。
- 一貫性-設計に矛盾があってはならない。矛盾を避けるために、設計が若干単純でなく、完全でなくてもよい。一貫性は正しさと同じくらい重要です。
- 完全性-設計は、現実的な範囲でできるだけ多くの重要な状況をカバーしなければなりません。合理的に予想されるすべてのケースをカバーしなければならない。単純化によって完全性を過度に低下させることは許されない。

これらが良い特性であることは、ほとんどの人が同意してくれると思います。このような設計哲学を用いることを、私はMITアプローチと呼ぶことにします。Common Lisp (CLOS付き)とSchemeは、MITアプローチの設計と実装を代表するものです。

worse-is-betterの思想はほんの少し違う。

- シンプルさ-実装とインターフェースの両方がシンプルであることが必要です。インターフェイスよりも実装がシンプルであることがより重要である。シンプルであることは、設計において最も重要な考慮事項である。
- 正しさ-設計は、観察可能なすべての側面において正しくなければならない。正しいことより単純であることの方が若干良い。
- 一貫性-設計は過度に矛盾してはならない。場合によっては一貫性を犠牲にして単純化することもできますが、実装の複雑さや一貫性の欠如を招くくらいなら、あまり一般的ではない状況に対処する部分を削除する方がよいでしょう。
- 完全性-設計は、現実的な範囲でできるだけ多くの重要な状況をカバーしなければならない。合理的に予想されるすべてのケースをカバーする必要があります。完全性は、他のどの品質よりも犠牲にすることができます。実際、実装の単純さが損なわれる場合は、完全性を犠牲にしなければならない。特にインターフェイスの一貫性は価値がない。

初期のUnixやCはこの流派の設計の例で、私はこの設計戦略の使用をニュージャージー・アプローチと呼ぶことにします。私は、明らかに悪い哲学であり、ニュージャージー・アプローチが悪いアプローチであることを納得させるために、意図的に「悪いことは良いこと」という哲学をカリカチュアライズしました。

しかし、私は、borse-is-betterは、たとえ藁人形であっても、the-right-thingよりも生存特性が優れており、ソフトウェアに使用する場合のニュージャージー・アプローチは、MITアプローチよりも良いアプローチであると信じています。

まず、MITとニュージャージーの区別が有効であり、それぞれの哲学の支持者が実際に自分の哲学が優れていると信じていることを示す物語を再話しよう。

MITとバークレー（ただしUnixの研究者）の2人の有名人が、オペレーティングシステムの問題を議論するために会ったことがあります。MITの人はITS(MIT AIラボのオペレーティング・システム)に詳しく、Unixのソースも読んでいたそうです。彼は、UnixがPCの敗者復活問題をどのように解決するかに興味を持っていました。

PCロスタイム問題は、ユーザープログラムがシステムルーチンを呼び出して、IOバッファのような重要な状態を持つ可能性のある長時間の処理を実行するときに発生します。操作中に割り込みが発生した場合、ユーザープログラムの状態を保存する必要があります。システムルーチンの呼び出しは通常1命令であるため、ユーザプログラムのPCはプロセスの状態を十分に把握することができません。システムルーチンは、バックアウトするか、フォワードを押さなければならない。正しいのは、バックアウトしてユーザプログラムのPCをシステムルーチンを呼び出した命令に戻し、例えば割り込み後のユーザプログラムの再開がシステムルーチンに再入力するようにすることである。PCを強制的にloserモードにすることからPC loser-ingと呼ばれる。loserはMITにおけるユーザーの愛称である。

MITの担当者は、このケースに対応したコードを見かけなかったので、ニュージャージー州の担当者に、この問題はどう処理されているのかと尋ねた。ニュージャージーの男は、Unixの人たちはこの問題を認識していたが、解決策はシステムルーチンが常に終了することだったが、時にはシステムルーチンがその動作を完了できなかったことを示すエラーコードが返されることがあった、と言った。正しいユーザープログラムは、このエラーコードをチェックして、単純にシステムルーチンをもう一度試すべきかどうかを判断しなければならない。MITの担当者は、この解決策が正しいものでないことを嫌った。

ニュージャージーの男は、Unixの設計思想はシンプルであり、正しいことは複雑すぎるから、Unixの解決策は正しいのだと言った。それに、プログラマーはこの余分なテストやループを簡単に挿入することができたのです。MITの人は、実装は単純だが、機能へのインターフェースが複雑だと指摘した。ニュージャージーの男は、Unix では正しいトレードオフが選択されてきた、つまり、実装の単純さはインターフェースの単純さよりも重要であると言いました。

MITの男は、時には柔らかいチキンを作るにはタフな男が必要だとつぶやきましたが、ニュージャージーの男には理解できませんでした(私もそう思います)。

さて、ここで私は、「borse-is-better」の方が良いということを主張したい。C言語はUnixを書くために設計されたプログラミング言語であり、ニュージャージー方式で設計されている。したがって、C言語はまともなコンパイラを書くのが容易な言語であり、プログラマはコンパイラが解釈しやすいテキストを書く必要がある。Cをファンシーなアセンブリ言語と呼ぶ人もいる。初期のUnixとCのコンパイラはどちらも単純な構造で、移植が容易で、実行に必要なマシンリソースも少なく、オペレーティングシステムやプログラミング言語に求めるものの50%から80%程度を提供するものでした。


どの時点でも存在するコンピュータの半分は中央値より悪い（小さいか遅い）です。UnixやC言語はそのようなコンピュータでも問題なく動作します。悪いほど良いという哲学は、実装の単純さが最も優先されるということであり、Unix や C はそのようなマシンへの移植が容易であることを意味します。したがって、もしUnixやCのサポートする50%の機能が満足できるものであれば、いたるところに出現し始めるだろうと予想される。そして、そうなったのですね。

UnixとCは究極のコンピュータウイルスなのだ。

ワース・イズ・ベター哲学のさらなる利点は、プログラマーが、良い性能と適度なリソースの使用を得るために、安全性や利便性、手間を多少犠牲にすることを条件付けられることです。ニュージャージー方式で書かれたプログラムは、小さなマシンでも大きなマシンでもうまく動くし、ウイルスの上に書かれたコードなので移植性も高い。

忘れてはならないのは、最初のウイルスが基本的に良いものでなければならないということです。そうであれば、ポータブルである限り、ウイルスの拡散は確実である。いったんウイルスが広まると、それを改良する圧力がかかるだろう。おそらく、機能を90%に近づけることで、ウイルスを改良しようとするだろうが、ユーザーはすでに正しいものよりも悪いものを受け入れるように仕向けられているのである。したがって、「悪いことは良いことだ」ソフトウェアは、まず受け入れられるようになり、次にユーザがより少ないものを期待するようになり、そして3番目にほぼ正しいものであるところまで改良されることになるのです。

具体的には、1987年当時のLispコンパイラがCコンパイラと同程度の性能であったとしても、Lispコンパイラを良くしたい人よりもCコンパイラを良くしたい人の方が多いのです。

良いニュースは、1995年には良いオペレーティングシステムとプログラミング言語が手に入るということですが、悪いニュースは、それらがUnixとC++になるということです。

悪いことは良いことである、というのは最後の利点です。ニュージャージーの言語とシステムは、複雑なモノリシックなソフトウェアを構築するほど強力ではないので、大規模なシステムはコンポーネントを再利用するように設計されなければなりません。したがって、統合の伝統が湧いてくるのです。

正しいものはどのように積み重ねられるのでしょうか？大きな複雑なシステムのシナリオとダイヤモンドのような宝石のシナリオの2つの基本シナリオがあります。

**大きな複雑なシステムのシナリオ**は、次のようなものです。

まず、正しいものが設計される必要があります。次に、その実装を設計する必要があります。最後にそれを実装する。それが正しいものであるため、望まれる機能をほぼ100%備えており、実装の簡素化には関心がなかったため、実装に長い時間がかかる。大きく、複雑である。正しく使うには、複雑なツールが必要です。最後の20%に80%の労力がかかるので、正しいものを出すのに長い時間がかかり、最も洗練されたハードウェアでしか満足に動かない。

**ダイヤモンドのような宝石のシナリオ**は、こんな感じです。

正しいものは設計に時間がかかるが、その途中のどの時点でも非常に小さいものである。高速に動作するように実装することは不可能であるか、ほとんどの実装者の能力を超えている。

この2つのシナリオは、Common LispとSchemeに対応する。最初のシナリオは、古典的な人工知能ソフトウェアのシナリオでもある。

正しいものはしばしばモノリシックなソフトウェアになりますが、それは正しいものがしばしばモノリシックに設計されているということ以外に理由はありません。つまり、この特性は偶然の産物なのです。

ここから得られる教訓は、正しいものを最初に目指すことはしばしば望ましくないということです。ウイルスのように広がるように、正しいものの半分を利用できるようにする方がよい。人々がそれに夢中になった後、時間をかけて90％の正しいものに改善するのです。

間違った教訓は、このたとえ話を文字通り受け取って、C言語がAIソフトウェアのための正しい手段であると結論づけることです。50％の解は基本的に正しくなければなりませんが、この場合はそうではありません。

しかし、LispコミュニティはLisp設計の立場を真剣に考え直す必要がある、という結論にしかなりません。これについては、後ほど詳しく述べたいと思います。

## 2.2 優れたLispプログラミングは難しい

Lispの愛好家の中には、Lispのプログラミングは簡単だと信じている人が少なくありません。これはある意味正しいことです。実際のアプリケーションを提供する際には、コードがうまく動作することが必要です。

C言語では、コンパイラが多くの記述を要求し、データ型が少ないため、プログラミングは常に困難です。Lispでは、非常に性能の悪いプログラムを書くことは非常に簡単ですが、Cではほとんど不可能です。以下に挙げる性能の悪いLispプログラムの例は、いずれも有能なLispプログラマが、配備を前提とした実際のアプリケーションを書く際に書いたものです。私はこれらを非常に悲しいと思います。


### 2.2.1 悪い宣言

この例は、やりがちなミスです。このプログラマは、配列の宣言を十分に行わなかったのです。そのため、各配列へのアクセスは、数命令で済むはずなのに、関数呼び出しと同じくらい遅かったのです。元の宣言は次のようなものでした。

```
(proclaim ’(type (array fixnum *) *ar1* *ar2* *ar3*))
```

3つの配列はたまたま固定サイズであったため，以下の正しい宣言に反映されています．

```
(proclaim ’(type (simple-array fixnum (4)) *ar1*))
(proclaim ’(type (simple-array fixnum (4 4)) *ar2*))
(proclaim ’(type (simple-array fixnum (4 4 4)) *ar3*))
```

不具合のある宣言を変更したところ、システム全体の性能が20％向上した。

### 2.2.2 実装の知識が乏しい

次の例は、一般的な機能の特定のケースについて実装が最適化されておらず、プログラマが高速になると思って一般的な機能を使用した場合です。ここでは、副作用の順番が重要な状況で、5つの値が返されています。

```
(multiple-value-prog1
  (values (f1 x)
          (f2 y)
          (f3 y)
          (f4 y)
          (f5 y))
  (setf (aref ar1 i1) (f6 y))
  (f7 x y))
```

この実装では、返り値が3つまでの場合、たまたまmultiple-value-prog1が最適化されますが、5つの値の場合はCONSesです。正しいコードは以下の通り。

```
(let ((x1 (f1 x))
      (x2 (f2 y))
      (x3 (f3 y))
      (x4 (f4 y))
      (x5 (f5 y)))
  (setf (aref ar1 i1) (f6 y))
  (f7 x y)
  (values x1 x2 x3 x4 x5))
```

この書き換えが必要であることをプログラマーが知っていなければならない理由はない。一方、パフォーマンスが期待通りでないことが分かったからといって、当該プログラマーのマネージャーが、Lispは間違った言語であると、彼のように結論づけることはないはずである。

### 2.2.3 FORTRANイディオムの使用

Common Lispコンパイラの中には、他のコンパイラと同じように最適化しないものがあります。次のような表現が使われることがあります。

```
(* -1 <form>)
```

というのも、コンパイラはしばしばこの変種に対してよりよいコードを生成するからです。

```
(<form>)
```

もちろん、1つ目はFORTRANイディオムのLispアナログである。

```
-1*<form>
```

### 2.2.4 まったくもって不適切なデータ構造

この例は信じられないと思われるかもしれません。これは私が見たいくつかのコードで実際に起こったことです。

```
(defun make-matrix (n m)
  (let ((matrix ()))
    (dotimes (i n matrix)
      (push (make-list m) matrix))))

(defun add-matrix (m1 m2)
  (let ((l1 (length m1))
        (l2 (length m2)))
    (let ((matrix (make-matrix l1 l2)))
      (dotimes (i l1 matrix)
        (dotimes (j l2)
          (setf (nth i (nth j matrix))
                (+ (nth i (nth j m1))
                   (nth i (nth j m2)))))))))
```

さらに悪いことに、このアプリケーションでは、行列はすべて固定サイズであり、LispでもFORTRANと同様に行列演算が高速に行えたはずです。

この例では、コードは非常に美しいのですが、行列を追加するのが遅いのです。したがって、これは優れたプロトタイプコードであり、お粗末なプロダクションコードである。Cではこれほどひどいプロダクションコードは書けない。

## 2.3 統合は神

悪いことは良いことだ。統合とは、.oファイルをリンクし、関数を自由に相互呼び出し、同じ基本データ表現を使用することである。外字ローダを持たず、関数呼び出しの境界を越えて型を強制せず、1つの言語を支配的にせず、実装技術の悩みをシステム全体に影響させないことです。

このような現実を前にすると、どんなに優れたLispの外国語機能であっても、単なるジョークに過ぎないのです。リスト上の全ての項目はLispの実装で対処可能です。これは、Lispの実装が正しいことの世界では行われてこなかっただけです。

ウイルスは生きているが、複雑な生物は死産である。Lispは適応しなければならないのであって、その逆ではありません。正しいことと2シリングで紅茶が飲める。

## 2.4 非Lisp環境はキャッチアップしている

これはなかなか直視できない。例えば、C言語の環境は、当初はLispの環境を模倣したものでしたが、今ではかなり良くなっています。現在の最高のC環境は次のようなものである。

- シンボリックデバッガ
- データインスペクタ
- ソースコードレベルでのシングルステップ
- 組み込み演算子に関するヘルプ
- ウィンドウベースのデバッギング
- シンボリックスタックバックトレース
- 構造体エディタ

そして、まもなくインクリメンタル・コンパイルとロードが可能になるでしょう。これらの環境は他の言語にも容易に拡張可能であり、多言語環境もそう遠くない将来に実現されるでしょう。

しかし、現在のLisp環境にはいくつかの欠点があります。第一に、ウィンドウベースの環境でありながら、うまく統合されていない傾向がある。つまり、関連する情報が関係性を伝えるように表現されていない。ウィンドウがたくさんあることは統合を意味しないし、同じ言語で実装され、同じイメージで動作することも意味しない。実際、現在利用可能なLisp環境では、深刻な統合がなされていないように思います。

第二に、永続的でないことです。一回のログインセッションのために定義されたようなものです。ファイルは永続的なデータを保持するために使われるもので、1960年代のものです。

第三に、外国語のインターフェースがあっても、多言語化されていない。

第四に、ソフトウェアのライフサイクルを広範に扱っていない。文書化、仕様書、メンテナンス、テスト、検証、修正、カスタマーサポートのすべてが無視されている。

第五に、情報が適切なタイミングでもたらされない。コンパイラはある程度の情報を提供できるが、何が完全に定義され、何が部分的に定義されているかは、環境側で概ね把握できるはずである。性能監視が面倒であってはならない。

第六に、環境の利用が難しいということです。知らなければならないことが多すぎる。仕組みを管理するのが難しすぎるのです。

第七に、興味深いソフトウェアのほとんどすべてがグループで書かれている現在、環境はマルチユーザーではありません。

本当の問題は、この10年間、Lisp環境においてほとんど進歩がなかったことです。

## 3.0 Lispが大勝利する方法

太陽が昇るとき、私は頂点に立つ。

下界で見上げている

ここに登ってくる途中

そこで待っている君を見ることになる。

君の隣に行く途中なんだ

今なら間に合うってわかるよ

**? & The Mysterians**

陰鬱な幕間は、ハッピーエンドになることもある。

## 3.1 Continue Standardization Progress

We need to bury our differences at the ISO level and realize that there is a short term need, which must be Common Lisp, and a long term need, which must address all the issues for practical applications.

We’ve seen that the right thing attitude has brought us a very large, complex-to-understand, and complex-to-implement Lisp—Common Lisp that solves way too many problems. We need to move beyond Common Lisp for the future, but that does not imply giving up on Common Lisp now. We’ve seen it is possible to do delivery of applications, and I think it is possible to provide tools that make it easier to write applications for deployment. A lot of work has gone into getting Common Lisp to the point of a right thing in many ways, and there are viable commercial implementations. But we need to solve the delivery and integration problems in spades.

Earlier I characterized the MIT approach as often yielding stillborn results. To stop Common Lisp standardization now is equivalent to abortion, and that is equivalent to the Lisp community giving up on Lisp. If we want to adopt the New Jersey approach, it is wrong to give up on Lisp, because C just isn’t the right language for AI.

It also simply is not possible to dump Common Lisp now, work on a new standard, and then standardize in a timely fashion. Common Lisp is all we have at the moment. No other dialect is ready for standardization.

Scheme is a smaller Lisp, but it also suffers from the MIT approach. It is too tight and not appropriate for large-scale software. At least Common Lisp has some facilities for that.

I think there should be an internationally recognized standard for Common Lisp. I don’t see what is to be gained by aborting the Common Lisp effort today just because it happens to not be the best solution to a commercial problem. For those who believe Lisp is dead or dying, what does killing off Common Lisp achieve but to convince people that the Lisp community kills its own kind? I wish less effort would go into preventing Common Lisp from becoming a standard when it cannot hurt to have several Lisp standards.

On the other hand, there should be a strong effort towards the next generation of Lisp. The worst thing we can do is to stand still as a community, and that is what is happening.

All interested parties must step forward for the longer-term effort.

## 3.2 Retain the High Ground in Environments

I think there is a mistake in following an environment path that creates monolithic environments. It should be possible to use a variety of tools in an environment, and it should be possible for those who create new tools to be able to integrate them into the environment.

I believe that it is possible to build a tightly integrated environment that is built on an open architecture in which all tools, including language processors, are protocol-driven. I believe it is possible to create an environment that is multi-lingual and addresses the software lifecycle problem without imposing a particular software methodology on its users.

Our environments should not discriminate against non-Lisp programmers the way existing environments do. Lisp is not the center of the world.

## 3.3 Implement Correctly

Even though Common Lisp is not structured as a kernel plus libraries, it can be implemented that way. The kernel and library routines can be in the form of .o files for easy linking with other, possibly non-Lisp, modules; the implementation must make it possible to write, for example, small utility programs. It is also possible to piggyback on existing compilers, especially those that use common back ends. It is also possible to implement Lisp so that standard debuggers, possibly with extensions, can be made to work on Lisp code.

It might take time for developers of standard tools to agree to extend their tools to Lisp, but it certainly won’t happen until our (exceptional) language is implementedmore like ordinary ones.

## 3.4 Achieve Total Integration

I believe it is possible to implement a Lisp and surrounding environment which has no discrimination for or against any other language. It is possible using multi-lingual environments, clever representations of Lisp data, conservative garbage collection, and conventional calling protocols to make a completely integrated Lisp that has no demerits.

## 3.5 Make Lisp the Premier Prototyping Language

Lisp is still the best prototyping language. We need to push this forward. A multi-lingual environment could form the basis or infrastructure for a multi-lingual prototyping system. This means doing more research to find new ways to exploit Lisp’s strengths and to introduce new ones.

Prototyping is the act of producing an initial implementation of a complex system. A prototype can be easily instrumented, monitored, and altered. Prototypes are often built from disparate parts that have been adapted to a new purpose. Descriptions of the construction of a prototype often involve statements about modifying the behavioral characteristics of an existing program. For example, suppose there exists a tree traversal program. The description of a prototype using this program might start out by saying something like

  Let S1 be the sequence of leaf nodes visited by P on tree T1 and S2 the leaf nodes visited by P on tree T2. Let C be a correspondence between S1 and S2 where f:S1 → S2 maps elements to corresponding elements.

Subsequent statements might manipulate the correspondence and use f. Once the definition of a leaf node is made explicit, this is a precise enough statement for a system to be able to modify the traversal routine to support the correspondence and f.

A language that describes the modification and control of an existing program can be termed a program language. Program languages be built on one or several underlying programming languages, and in fact can be implemented as part of the functionality of the prototyping environment. This view is built on the insight that an environment is a mechanism to assist a programmer in creating a working program, including preparing the source text. There is no necessary requirement that an environment be limited to working only with raw source text. As another example, some systems comprise several processes communicating through channels. The creation of this part of the system can be visual, with the final result produced by the environment being a set of source code in several languages, build scripts, link directives, and operating system calls. Because no single programming language encompasses the program language, one could call such a language an epi-language.

## 3.6 The Next Lisp

I think there will be a next Lisp. This Lisp must be carefully designed, using the principles for success we saw in worse-is-better.

There should be a simple, easily implementable kernel to the Lisp. That kernel should be both more than Scheme— modules and macros—and less than Scheme—continuations remain an ugly stain on the otherwise clean manuscript of Scheme.

The kernel should emphasize implementational simplicity, but not at the expense of interface simplicity. Where one conflicts with the other, the capability should be left out of the kernel. One reason is so that the kernel can serve as an extension language for other systems, much as GNU Emacs uses a version of Lisp for defining Emacs macros.

Some aspects of the extreme dynamism of Common Lisp should be reexamined, or at least the tradeoffs reconsidered. For example, how often does a real program do this?

```
(defun f ...)
(dotimes (...)
  ...
  (setf (symbol-function ’f) #’(lambda ...)) ...)
```

Implementations of the next Lisp should not be influenced by previous implementations to make this operation fast, especially at the expense of poor performance of all other function calls.

The language should be segmented into at least four layers:

1. The kernel language, which is small and simple to implement. In all cases, the need for dynamic redefinition should be re-examined to determine that support at this level is necessary. I believe nothing in the kernel need be dynamically redefinable.
2. A linguistic layer for fleshing out the language. This layer may have some implementational difficulties, and it will probably have dynamic aspects that are too expensive for the kernel but too important to leave out.
3. A library. Most of what is in Common Lisp would be in this layer.
4. Environmentally provided epilinguistic features.

In the first layer I include conditionals, function calling, all primitive data structures, macros, single values, and very basic object-oriented support.

In the second layer I include multiple values and more elaborate object-oriented support. The second layer is for difficult programming constructs that are too important to leave to environments to provide, but which have sufficient semantic consequences to warrant precise definition. Some forms of redefinition capabilities might reside here.

In the third layer I include sequence functions, the elaborate IO functions, and anything else that is simply implemented in the first and possibly the second layers.

These functions should be linkable.

In the fourth layer I include those capabilities that an environment can and should provide, but which must be standardized. A typical example is defmethod from CLOS. In CLOS, generic functions are made of methods, each method applicable to certain classes. The first layer has a definition form for a complete generic function -that is, for a generic function along with all of its methods, defined in one place (which is how the layer 1 compiler wants to see it). There will also be means of associating a name with the generic function. However, while developing a system, classes will be defined in various places, and it makes sense to be able to see relevant (applicable) methods adjacent to these classes. defmethod is the construct to define methods, and defmethod forms can be placed anywhere amongst other definitional forms.

But methods are relevant to each class on which the method is specialized, and also to each subclass of those classes. So, where should the unique defmethod form be placed? The environment should allow the programmer to see the method definition in any or all of these places, while the real definition should be in some particular place. That place might as well be in the single generic function definition form, and it is up to the environment to show the defmethod equivalent near relevant classes when required, and to accept as input the source in the form of a defmethod (which it then places in the generic function definition).

We want to standardize the defmethod form, but it is a linguistic feature provided by the environment. Similarly, many uses of elaborate lambda-list syntax, such as keyword arguments, are examples of linguistic support that the environment can provide possibly by using color or other adjuncts to the text.

In fact, the area of function-function interfaces should be re-examined to see what sorts of argument naming schemes are needed and in which layer they need to be placed.

Finally, note that it might be that every layer 2 capability could be provided in a layer 1 implementation by an environment.

## 3.7 Help Application Writers Win

The Lisp community has too few application writers. The Lisp vendors need to make sure these application writers win. To do this requires that the parties involved be open about their problems and not adversarial. For example, when an expert system shell company finds problems, it should open up its source code to the Lisp vendor so that both can work towards the common goal of making a faster, smaller, more deliverable product. And the Lisp vendors should do the same.

The business leadership of the AI community seems to have adopted the worst caricature-like traits of business practice: secrecy, mistrust, run-up-the-score competitiveness. We are an industry that has enough common competitors without searching for them among our own ranks.

Sometimes the sun also rises.

References

[1] ? & the Mysterians, 96 Tears, Pa-go-go Records 1966, re-released on Cameo Records, September 1966.







-----

1. Adapting the equation to a more normal mix of instructions involves only changing the value of Pft, and it is not unusual for the adjusted Pft to lie in the range of 100 to 1000.
2. This is in contrast to a language such as C where the default is a sort of “machine-independent”’ assembly language, and including other tools requires an explicit use of the appropriate library. Of course, this may lead to a more deliberate style of programming, but it is at the cost of making the programmer’s job more tedious.
3. The name Treeshaker is meant to be evocative of the idea of actually shaking a tree to dislodge dead branches or other trash.




