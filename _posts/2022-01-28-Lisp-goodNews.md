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

### 1.6.2 The Reorganizer

For programs whose working set exceeds available real memory, the time spent inside virtual memory system-code can greatly exceed the time spent in user-code. For example, let us assume that we have a program which does nothing but memory-reference instructions randomly throughout its working set. If we denote by WS the working set of a program, by Mem the real memory available to that program, and by Utime the time spent in actual user-code, and by Total the total run time for the program, a simple analysis gives the following equations for Total:

Total = Utime, for WS ≤ Mem
Total = Utime+Pft⋅(WS–Mem)⁄(WS),for WS>Mem

Pft is the page-fault-time—that is, amount of time it takes to handle a page-fault. On typical machines this number is usually 1,000 to 10,000 times as long as it takes to execute a memory-reference instruction that doesn’t cause a page-fault.(*1)

Because a Lisp program can often have a working set exceeding available memory, tools that help reduce working set can lead to very large performance improvements. In some cases, the working set can be reduced by lessening reliance on dynamically allocated objects—that is, by lessening CONSing of the program. However, in instances where the working set size is due primarily to references to permanent objects, other techniques are needed.

The approach taken in the Lucid Delivery Tool Kitis to reorganize the permanent objects in the Lisp address space so as to place objects that have similar patterns of reference at nearby addresses. (In particular, this means at least segregating the objects that are referenced by a program from those that are not.)

### 1.6.3 The Treeshaker

Most Lisp development systems, including Lucid’s, provide all the resources of the Lisp system by default, and this in turn leads to a style of development in which the programmer makes use of whatever tool happens to be most convenient.(*2) Because much of the basic Lisp system (or any development system built on top of the basic Lisp system) will generally be unused by a given application, it is very worthwhile to have a tool for excising these unused parts. This tool is called the Treeshaker.(*3)

Treeshaker execution occurs in three phases: walking, testing and writing. In the walking phase, the Treeshaker accumulates a set of objects that need to be included in the saved image. After making this set, the treeshaker runs a test of the application to check that all objects which are used in a typical run have been included. The writing phase then generates an executable image which will run the application.

To a first approximation, the walk phase is just a matter of computing the connected component of the Lisp image (treated as a directed graph in the obvious way) generated by the application’s toplevel function. However, because of the way that Lisp objects are generally connected this usually includes almost the entire Lisp image including the unused subsystems. Therefore the treeshaker uses several techniques to find connections between objects that do not actually need to be followed in the walk.


### 1.6.4 Results

The first example is a simple expert system that helps align magnetic resonance imaging equipment. The results are for the original program, and a reorganized and treeshaken version running two different tests, a short and a long test. WS is the working set in megabytes. The time is expressed as hours:minutes.

MRI Alignment Program
|  |Short|Long|Size|WS|
|:-|:---|:----|:---|:--|
|Original|2:22|*:**|4.45mb|3.50mb|
|Final|0:42|4:11|1.65mb|2.25mb|

The second example is Reduce, a symbolic algebra system. The three programs are the original, a treeshaken version, and a treeshaken and reorganized version. The CPU column is the time in seconds the CPU spent executing user code—that is, all other time is page fault handling time.

Reduce Computer Algebra System
|   |Size|Time|Faults|CPU|
|:--|:---|:---|:-----|:--|
|Original|6.68mb|6:20|1230|55.2|
|Shaken|2.78mb|3:25|680|52.1|
|Reorganized|2.78mb|1:50|220|50.1|

## 2.0 Lisp’s Apparent Failures

Too many teardrops for one heart to be crying.

Too many teardrops for one heart to carry on.

You’re way on top now, since you left me,

Always laughing, way down at me.

**? & The Mysterians**

This happy story, though, has a sad interlude, an interlude that might be attributed to the failure of AI to soar, but which probably has some other grains of truth that we must heed. The key problem with Lisp today stems from the tension between two opposing software philosophies. The two philosophies are called The Right Thing and Worse is Better.

## 2.1 The Rise of Worse is Better
I and just about every designer of Common Lisp and CLOS has had extreme exposure to the MIT/Stanford style of design. The essence of this style can be captured by the phrase **the right thing**. To such a designer it is important to get all of the following characteristics right:

Simplicity—the design must be simple, both in implementation and interface. It is more important for the interface to be simple than the implementation.
Correctness—the design must be correct in all observable aspects. Incorrectness is simply not allowed.
Consistency—the design must not be inconsistent. A design is allowed to be slightly less simple and less complete to avoid inconsistency. Consistency is as important as correctness.
Completeness—the design must cover as many important situations as is practical. All reasonably expected cases must be covered. Simplicity is not allowed to overly reduce completeness.

I believe most people would agree that these are good characteristics. I will call the use of this philosophy of design the MIT approach. Common Lisp (with CLOS) and Scheme represent the MIT approach to design and implementation.

The worse-is-better philosophy is only slightly different:

Simplicity—the design must be simple, both in implementation and interface. It is more important for the implementation to be simple than the interface.
Simplicity is the most important consideration in a design.
Correctness—the design must be correct in all observable aspects. It is slightly better to be simple than correct.
Consistency—the design must not be overly inconsistent. Consistency can be sacrificed for simplicity in some cases, but it is better to drop those parts of the design that deal with less common circumstances than to introduce either implementational complexity or inconsistency.
Completeness—the design must cover as many important situations as is practical. All reasonably expected cases should be covered. Completeness can be sacrificed in favor of any other quality. In fact, completeness must sacrificed whenever implementation simplicity is jeopardized. Consistency can be sacrificed to achieve completeness if simplicity is retained; especially worthless is consistency of interface.

Early Unix and C are examples of the use of this school of design, and I will call the use of this design strategy the New Jersey approach I have intentionally caricatured the worse-is-better philosophy to convince you that it is obviously a bad philosophy and that the New Jersey approach is a bad approach.

However, I believe that worse-is-better, even in its strawman form, has better survival characteristics than the-right-thing, and that the New Jersey approach when used for software is a better approach than the MIT approach.

Let me start out by retelling a story that shows that the MIT/New-Jersey distinction is valid and that proponents of each philosophy actually believe their philosophy is better.

Two famous people, one from MIT and another from Berkeley (but working on Unix) once met to discuss operating system issues. The person from MIT was knowledgeable about ITS (the MIT AI Lab operating system) and had been reading the Unix sources. He was interested in how Unix solved the PC loser-ing problem.

The PC loser-ing problem occurs when a user program invokes a system routine to perform a lengthy operation that might have significant state, such as IO buffers. If an interrupt occurs during the operation, the state of the user program must be saved. Because the invocation of the system routine is usually a single instruction, the PC of the user program does not adequately capture the state of the process. The system routine must either back out or press forward. The right thing is to back out and restore the user program PC to the instruction that invoked the system routine so that resumption of the user program after the interrupt, for example, re-enters the system routine. It is called PC loser-ing because the PC is being coerced into loser mode, where loser is the affectionate name for user at MIT.

The MIT guy did not see any code that handled this case and asked the New Jersey guy how the problem was handled. The New Jersey guy said that the Unix folks were aware of the problem, but the solution was for the system routine to always finish, but sometimes an error code would be returned that signaled that the system routine had failed to complete its action. A correct user program, then, had to check the error code to determine whether to simply try the system routine again. The MIT guy did not like this solution because it was not the right thing.

The New Jersey guy said that the Unix solution was right because the design philosophy of Unix was simplicity and that the right thing was too complex. Besides, programmers could easily insert this extra test and loop. The MIT guy pointed out that the implementation was simple but the interface to the functionality was complex. The New Jersey guy said that the right tradeoff has been selected in Unix—namely, implementation simplicity was more important than interface simplicity.

The MIT guy then muttered that sometimes it takes a tough man to make a tender chicken, but the New Jersey guy didn’t understand (I’m not sure I do either).

Now I want to argue that worse-is-better is better. C is a programming language designed for writing Unix, and it was designed using the New Jersey approach. C is therefore a language for which it is easy to write a decent compiler, and it requires the programmer to write text that is easy for the compiler to interpret. Some have called C a fancy assembly language. Both early Unix and C compilers had simple structures, are easy to port, require few machine resources to run, and provide about 50%–80% of what you want from an operating system and programming language.


Half the computers that exist at any point are worse than median (smaller or slower). Unix and C work fine on them. The worse-is-better philosophy means that implementation simplicity has highest priority, which means Unix and C are easy to port on such machines. Therefore, one expects that if the 50% functionality Unix and C support is satisfactory, they will start to appear everywhere. And they have, haven’t they?

Unix and C are the ultimate computer viruses.

A further benefit of the worse-is-better philosophy is that the programmer is conditioned to sacrifice some safety, convenience, and hassle to get good performance and modest resource use. Programs written using the New Jersey approach will work well both in small machines and large ones, and the code will be portable because it is written on top of a virus.

It is important to remember that the initial virus has to be basically good. If so, the viral spread is assured as long as it is portable. Once the virus has spread, there will be pressure to improve it, possibly by increasing its functionality closer to 90%, but users have already been conditioned to accept worse than the right thing. Therefore, the worse-is-better software first will gain acceptance, second will condition its users to expect less, and third will be improved to a point that is almost the right thing.

In concrete terms, even though Lisp compilers in 1987 were about as good as C compilers, there are many more compiler experts who want to make C compilers better than want to make Lisp compilers better.

The good news is that in 1995 we will have a good operating system and programming language; the bad news is that they will be Unix and C++.

There is a final benefit to worse-is-better. Because a New Jersey language and system are not really powerful enough to build complex monolithic software, large systems must be designed to reuse components. Therefore, a tradition of integration springs up.

How does the right thing stack up? There are two basic scenarios: the big complex system scenario and the diamond-like jewel scenario.

The **big complex system scenario** goes like this:

First, the right thing needs to be designed. Then its implementation needs to be designed. Finally it is implemented. Because it is the right thing, it has nearly 100% of desired functionality, and implementation simplicity was never a concern so it takes a long time to implement. It is large and complex. It requires complex tools to use properly. The last 20% takes 80% of the effort, and so the right thing takes a long time to get out, and it only runs satisfactorily on the most sophisticated hardware.

The **diamond-like jewel scenario** goes like this:

The right thing takes forever to design, but it is quite small at every point along the way. To implement it to run fast is either impossible or beyond the capabilities of most implementors.

The two scenarios correspond to Common Lisp and Scheme. The first scenario is also the scenario for classic artificial intelligence software.

The right thing is frequently a monolithic piece of software, but for no reason other than that the right thing is often designed monolithically. That is, this characteristic is a happenstance.

The lesson to be learned from this is that it is often undesirable to go for the right thing first. It is better to get half of the right thing available so that it spreads like a virus. Once people are hooked on it, take the time to improve it to 90% of the right thing.

A wrong lesson is to take the parable literally and to conclude that C is the right vehicle for AI software. The 50% solution has to be basically right, and in this case it isn’t.

But, one can conclude only that the Lisp community needs to seriously rethink its position on Lisp design. I will say more about this later.

## 2.2 Good Lisp Programming is Hard

Many Lisp enthusiasts believe that Lisp programming is easy. This is true up to a point. When real applications need to be delivered, the code needs to perform well.

With C, programming is always difficult because the compiler requires so much description and there are so few data types. In Lisp it is very easy to write programs that perform very poorly; in C it is almost impossible to do that. The following examples of badly performing Lisp programs were all written by competent Lisp programmers while writing real applications that were intended for deployment. I find these quite sad.


### 2.2.1 Bad Declarations

This example is a mistake that is easy to make. The programmer here did not declare his arrays as fully as he could have. Therefore, each array access was about as slow as a function call when it should have been a few instructions. The original declaration was as follows:

```
(proclaim ’(type (array fixnum *) *ar1* *ar2* *ar3*))
```

The three arrays happen to be of fixed size, which is reflected in the following correct declaration:

```
(proclaim ’(type (simple-array fixnum (4)) *ar1*))
(proclaim ’(type (simple-array fixnum (4 4)) *ar2*))
(proclaim ’(type (simple-array fixnum (4 4 4)) *ar3*))
```

Altering the faulty declaration improved the performance of the entire system by 20%.

### 2.2.2 Poor Knowledge of the Implementation

The next example is where the implementation has not optimized a particular case of a general facility, and the programmer has used the general facility thinking it will be fast. Here five values are being returned in a situation where the order of side effects is critical:

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

The implementation happens to optimize multiple-value-prog1 for up to three return values, but the case of five values CONSes. The correct code follows:

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

There is no reason that a programmer should know that this rewrite is needed. On the other hand, finding that performance was not as expected should not have led the manager of the programmer in question to conclude, as he did, that Lisp was the wrong language.

### 2.2.3 Use of FORTRAN Idioms

Some Common Lisp compilers do not optimize the same way as others. The following expression is sometimes used:

```
(* -1 <form>)
```

when compilers often produce better code for this variant:

```
(<form>)
```

Of course, the first is the Lisp analog of the FORTRAN idiom:

```
-1*<form>
```

### 2.2.4 Totally Inappropriate Data Structures

Some might find this example hard to believe. This really occurred in some code I’ve seen:

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

What’s worse is that in the particular application, the matrices were all fixed size, and matrix arithmetic would have been just as fast in Lisp as in FORTRAN.

This example is bitterly sad: The code is absolutely beautiful, but it adds matrices slowly. Therefore it is excellent prototype code and lousy production code. You know, you cannot write production code asbadasthisinC.

## 2.3 Integration is God

In the worse-is-better world, integration is linking your .o files together, freely intercalling functions, and using the same basic data representations. You don’t have a foreign loader, you don’t coerce types across function-call boundaries, you don’t make one language dominant, and you don’t make the woes of your implementation technology impact the entire system.

The very best Lisp foreign functionality is simply a joke when faced with the above reality. Every item on the list can be addressed in a Lisp implementation. This is just not the way Lisp implementations have been done in the right thing world.

The virus lives while the complex organism is stillborn. Lisp must adapt, not the other way around. The right thing and 2 shillings will get you a cup of tea.

## 2.4 Non-Lisp Environments are Catching Up

This is hard to face up to. For example, most C environments—initially imitative of Lisp environments—are now pretty good. Current best C environments have the following:

symbolic debuggers
data inspectors
source code level single stepping
help on builtin operators
window-based debugging
symbolic stack backtraces
structure editors

And soon they will have incremental compilation and loading. These environments are easily extendible to other languages, with multi-lingual environments not far behind.

Though still the best, current Lisp environments have several prominent failures. First, they tend to be window-based but not well integrated. That is, related information is not represented so as to convey the relationship. A multitude of windows does not mean integration, and neither does being implemented in the same language and running in the same image. In fact, I believe no currently available Lisp environment has any serious amount of integration.

Second, they are not persistent. They seemed to be defined for a single login session. Files are used to keep persistent data—how 1960’s.

Third, they are not multi-lingual even when foreign interfaces are available.

Fourth, they do not address the software lifecycle in any extensive way. Documentation, specifications, maintenance, testing, validation, modification, and customer support are all ignored.

Fifth, information is not brought to bear at the right times. The compiler is able to provide some information, but the environment should be able to generally know what is fully defined and what is partially defined. Performance monitoring should not be a chore.

Sixth, using the environment is difficult. There are too many things to know. It’s just too hard to manage the mechanics.

Seventh, environments are not multi-user when almost all interesting software is now written in groups.

The real problem has been that almost no progress in Lisp environments has been made in the last 10 years.

## 3.0 How Lisp Can Win Big

When the sun comes up, I’ll be on top.

You’re right down there looking up.

On my way to come up here,

I’m gonna see you waiting there.

I’m on my way to get next to you.

I know now that I’m gonna get there.

**? & The Mysterians**

The gloomy interlude can have a happy ending.

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




