---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: ASP.NET Web Pages の概要 - プログラミングの基本事項 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルによって、プログラムでは、ASP.NET Web ページ Razor 構文を使用する方法の概要がわかります。 学習内容。プル要求に使用する基本的な構文 'Razor'.
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: ec1c055d1b3f6ca5c6374a18840c2595bb368e0e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034559"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>ASP.NET Web Pages のプログラミングの基礎の概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルによって、プログラムでは、ASP.NET Web ページ Razor 構文を使用する方法の概要がわかります。
> 
> 学習内容。
> 
> - ASP.NET Web Pages でのプログラミングで使用する基本的な"Razor"構文。
> - いくつか基本的な C# を使用するプログラミング言語であります。
> - Web ページの基本的なプログラミング概念がいくつか。
> - サイトを使用するパッケージ (事前作成されたコードが含まれているコンポーネント) をインストールする方法。
> - 使用する*ヘルパー*一般的なプログラミング タスクを実行します。
>   
> 
> 説明した機能/テクノロジ:
> 
> - NuGet パッケージ マネージャー.
> - `Gravatar`ヘルパー。


このチュートリアルは、主にで使用する ASP.NET Web Pages のプログラミングの構文を紹介します。 について学習*Razor 構文*c# で記述されたコードのプログラミング言語とします。 前のチュートリアルでこの構文の glimpse に関する情報を取得しました。このチュートリアルでは、構文の詳細について説明します。

このチュートリアルは、ほとんどのプログラミングで 1 つのチュートリアルでは、表示されるとは、唯一のチュートリアルであることをお約束*のみ*プログラミングの概要。 このセットの残りのチュートリアルでは、実際に興味深い処理を実行するページを作成します。

についても学習*ヘルパー*します。 ヘルパーは、コンポーネント: コードのパッケージの一部 — ページに追加することができます。 ヘルパーは、する可能性のある場合は面倒なまたは手動で行う複雑な作業を実行します。

## <a name="creating-a-page-to-play-with-razor"></a>Razor で再生するページの作成

このセクションでは再生することを少し Razor での基本的な構文を取得できるように。

実行されていない場合は、WebMatrix を起動します。 前のチュートリアルで作成した web サイトを使用します ([Web ページの開始を取得する](https://go.microsoft.com/fwlink/?LinkId=251578))。 をクリックして、再度開きます、**個人用サイト**選択**WebPageMovies**:

![WebMatrix start 画面で強調表示されている個人用サイト、サイトを開くオプションを表示](intro-to-web-pages-programming/_static/image1.png)

選択、**ファイル**ワークスペース。

リボンで、次のようにクリックします。**新規**ページを作成します。 選択**CSHTML**し名前を新しいページ*TestRazor.cshtml*します。

**[OK]** をクリックします。

新機能が既に完全に置き換え、ファイルに、次をコピーします。

> [!NOTE]
> コピーしたコードまたはマークアップの例は、ページに、インデントや配置しないする場合、チュートリアルと同じです。 インデントや配置は、コードの実行方法、ただしに影響しません。


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>ページの例を確認します。

表示される内容のほとんどは、通常の HTML です。 ただし、上部にあるが、このコード ブロックがあります。

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

このコード ブロックの詳細については、次のことに注意してください。

- @ 文字 ASP.NET に指示次に、HTML ではなく、Razor コードを示します。 ASP.NET には、以降後のすべての処理は、@ 文字コードをいくつかの HTML にもう一度実行するまでとします。 (この場合は、 &lt;!DOCTYPE&gt;要素。
- 中かっこ ({および}) コードには、複数の行がある場合は、Razor コード ブロックを囲みます。 中かっこは、そのブロックのコードの開始し、終了位置に、ASP.NET を伝えます。
- /文字/mark コメント、実行されないコードの一部では、します。
- 各ステートメントは、セミコロン (;) で終わる必要があります。 (ないコメント、ただし。)
- 内の値を格納する*変数*、作成する (*宣言*) キーワード var 関数で 変数を作成するときに名前を付けますを文字、数字、およびアンダー スコアを含めることができますが (\_)。 変数の名前は、先頭を数字にすることはできず、(var) などのプログラミングのキーワードの名前を使用することはできません。
- ("ASP.NET"や「Web ページ」) などの文字の文字列を引用符で囲みます。 (二重引用符をある必要があります。)数値は、引用符ではありません。
- 引用符の外部での空白は重要でありません。 改行がほとんど重要でないです。この例外は、行の間で引用符で囲まれた文字列を分割することはできません。 インデントや配置があっても問題ありません。

この例から明らかでないものはすべてのコードが大文字小文字を区別します。 つまり、変数の超えていますが超えていますまたは超えていますという名前の変数よりも、別の変数であります。 同様に、var は、キーワードが Var ことはできません。

### <a name="objects-and-properties-and-methods"></a>オブジェクトのプロパティおよびメソッド

これは、DateTime.Now 式です。 簡単に言えば、DateTime は、*オブジェクト*します。 オブジェクトをプログラミングできるものとは、ページ、テキスト ボックス、ファイル、画像、web 要求を電子メール メッセージ、顧客レコードなど。オブジェクトが 1 つ以上ある*プロパティ*その特性を記述します。 テキスト ボックスのオブジェクトがテキスト プロパティ (特に)、要求オブジェクトが Url プロパティ (など)、電子メール メッセージは、From プロパティ、To プロパティ、これにします。 オブジェクトでもある*メソッド*を実行できる「動詞」であります。 多くのオブジェクトを操作できるようになります。

この例からわかる、DateTime、プログラムの日付と時刻をできるオブジェクトです。 現在の日付と時刻を返すようになりましたという名前のプロパティがあります。

### <a name="using-code-to-render-markup-in-the-page"></a>ページのマークアップを表示するためにコードを使用します。

ページの本文には、次に注意してください。

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

ここでも、@ 文字 ASP.NET に指示に従ってその内容は HTML ではなく、コード。 マークアップを追加できますの後にコード式、および ASP.NET では、その式の右側の値をその時点でレンダリングされます。 例では、@aという名前の変数の値は、内容を表示、@productと変数の名前付きの製品では、内容を表示します。

ない場合、変数に制限されていますが。 ここでは、いくつかのインスタンスで、@ 文字式の前に。

- @(、\*b)、変数内のあらゆるものの製品を表示、および b。 (、\*演算子が乗算を意味します)。
- @(テクノロジ +""+ 製品) それらを連結し、間にスペースを追加した後、変数のテクノロジと製品内の値を表示します。 演算子 (+) 文字列を連結する場合は、演算子と同じ番号を追加します。 ASP.NET は、通常は数値または文字列に作業しているしで適切な処理はかどうかを確認する、+ 演算子。
- @Request.Url 要求オブジェクトの Url プロパティを表示します。 要求オブジェクトには、ブラウザーから現在の要求に関する情報が含まれていて、もちろん Url プロパティがその現在の要求の URL が含まれます。

例を実行できるは、さまざまな方法で機能を表示するのにも設計されています。 上部にあるコード ブロック内の計算を行う、変数に、その結果を格納、およびマークアップ内の変数をレンダリングすることができます。 または、マークアップで、式の右側での計算を行うことができます。 使用する方法で何をしていると、ある程度は、独自の基本設定に依存します。

### <a name="seeing-the-code-in-action"></a>コードの動作が表示されます。

ファイルの名前を右クリックして **ブラウザーで起動**します。 すべての値と解決 ページで式を使用してブラウザーでページを参照してください。

![ブラウザーで実行されている 'TestRazor' ページ](intro-to-web-pages-programming/_static/image2.png)

ブラウザーでソースを確認します。

![ブラウザーでページ ソースを 'test Razor'](intro-to-web-pages-programming/_static/image3.png)

前のチュートリアルではご想像のとおり、ページの Razor コードが。 表示は、実際の表示値です。 ページを実行するときに、WebMatrix に組み込まれている web サーバーに実際に要求を作成しています。 要求が受信したときに、ASP.NET は、すべての値と式を解決し、ページにその値を表示します。 ページをブラウザーに送信します。

> [!TIP] 
> 
> **Razor および c#**
> 
> ここまでは、Razor 構文を使用していることをお話します。 これは、とおりですが、完全なストーリーではありません。 使用する実際のプログラミング言語と呼びます*c#* します。 C# 10 年以上前に、Microsoft によって作成され、Windows アプリを作成するための主なプログラミング言語の 1 つになりました。 変数の名前を付ける方法とこれにステートメントを作成する方法について説明しましたすべてのルールです。 実際には、c# 言語のすべての規則。
> 
> Razor より具体的には、小さなページにこのコードを埋め込む方法の規則セットを指します。 を使用して、ページ内のコードをマークしてを使用しての規則ではたとえば、@ {} コード ブロックを埋め込むには、ページの Razor 部分を表す。 ヘルパーは Razor の一部としても考慮します。 Razor 構文は、ASP.NET Web Pages でだけよりもより多くの場所で使用されます。 (たとえば、これがで使用も ASP.NET MVC のビュー。)
> 
> こんなため、プログラミングの ASP.NET Web Pages についての情報を探す場合 Razor への参照の多くがわかります。 ただし、これらの参照の多くは、実行しているを確認し、そのため混乱させるように適用されません。 実際には、プログラミングに関する多くは本当にしようとする操作 (C#) または ASP.NET の操作のいずれかに注意します。 ように、具体的には Razor に関する情報を検索する場合に、必要な回答を検索しない可能性があります。


## <a name="adding-some-conditional-logic"></a>いくつかの条件付きロジックを追加します。

ページ内のコードの使用の詳細については、優れた機能の 1 つは、さまざまな条件に基づいて動作を変更することができます。 このチュートリアルでは、ページに表示される内容を変更する方法をいろいろします。

例は、シンプルでいくらかの条件付きロジックに集中できますように不自然になります。 作成します ページはこの操作を行います。

- 最初にあるかどうかに応じて、ページが表示されますまたはページを送信するためのボタンをクリックしたかどうかは、ページで別のテキストを表示します。 最初の条件付きテストになります。
- 特定の値が (http://...?show=true) の URL のクエリ文字列で渡された場合にのみ、メッセージを表示します。 2 番目の条件付きテストになります。

WebMatrix で、ページを作成し、名前を付けます*TestRazorPart2.cshtml*します。 (リボンで、次のようにクリックします**新規**、選択**CSHTML**で、ファイルの名前をクリック**OK**。)。

次のように、そのページの内容を置き換えます。

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

上部にあるコード ブロックでは、テキスト メッセージをという名前の変数を初期化します。 内部に、ページの本文のメッセージ変数の内容が表示されます、 &lt;p&gt;要素。 マークアップも含まれています、&lt;入力&gt;要素を作成する、**送信**ボタンをクリックします。

今すぐ動作を確認するページを実行します。 ここでは、基本的には、静的なページをクリックした場合でも、**送信**ボタンをクリックします。

WebMatrix に戻ります。 コード ブロック内では、次の強調表示されたコードを追加*後*メッセージを初期化します線。

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>場合は、{} ブロック

If を追加した内容が条件です。 コードで、if の条件があるこのような構造体。

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

テストする条件は、かっこ内に示します。 これには値または true または false を返す式。 条件が true の場合、ASP.NET は、ステートメントまたは中かっこ内にあるステートメントを実行します。 (これらは、*し*の一部、*の if-then*ロジック)。条件が false の場合は、コードのブロックがスキップされます。

ここでは if でテストできる条件のいくつかの例のステートメント。

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

変数に対して値または式を使用してテストすることができます、<em>論理演算子</em>または<em>比較演算子</em>: より大きい (= =)、等号 (&gt;) より小さい (&lt;)、大きいまたは等しい (&gt;=) とに等しいまたはそれよりも小さい (&lt;=)。 ! = 演算子の意味が等しくない: などの場合 (、! = 0) ことを意味<em>場合</em> <em>、</em><em>が 0 と等しく</em>。

> [!NOTE]
> 等号 (= =) 比較演算子がない = と同じことを確認することを確認します。 = 演算子は値を割り当てる場合にのみ使用 (var を = 2)。 これらの演算子が混在する場合は、エラーが発生しますかまたはいくつかの予期しない結果が表示されます。


完全な構文は、何かが true かどうかをテストする if(IsDone == true) します。 ショートカット if(IsDone) を使用することもできます。 比較演算子がない場合は、ASP.NET では、真の場合はテストしていること前提としています。

します。 演算子自体では、論理 NOT を意味します。 たとえば、条件 if (!IsPost) 手段*IsPost が true でない場合*します。

論理 AND を使用して条件を結合することができます (&amp; &amp;演算子) または論理 OR (| | 演算子)。 上記の例での場合は、の最後の条件など*true AND displayMessage が false に設定されている FileProcessingIsDone に設定されていない場合*します。

### <a name="the-else-block"></a>Else ブロック

最後に場合に関するブロック: ブロックの後に else ブロック場合。 Else ブロックが役には、条件が false の場合は、別のコードを実行することです。 簡単な例を次に示します。

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

このシリーズの他のブロックを使用して、便利な以降のチュートリアルでは、いくつかの例を確認します。

### <a name="testing-whether-the-request-is-a-submit-post"></a>要求が送信 (post) であるかどうかをテストします。

さらに多くが、条件 if(IsPost) {...} には、例に戻りましょう。 IsPost は、実際には、現在のページのプロパティです。 初めてページが要求されると、IsPost false を返します。 ただし、ボタンをクリックします。 または、それ以外の場合、ページを送信する場合は、投稿のは、-IsPost が true を返します。 したがって IsPost には、フォームの送信を扱っているかどうかを判断することができます。 (HTTP の動詞の観点から、要求が GET 操作では、IsPost false を返します。 要求が POST 操作、IsPost true を返します。)後のチュートリアルでは、このテストは特に便利ですが、入力フォームで操作します。

ページを実行します。 要求したため、これは、最初に「、ページを要求した最初の時間は、」を表示するページ。 その文字列は、メッセージ変数を初期化する値です。 If(IsPost) テストがため場合は、内部のコード ブロックを返す現時点では、false は実行されません。

をクリックして、**送信**ボタンをクリックします。 ページを再度要求します。 として、前に、メッセージ変数に設定されます"This is 初めて..."です。 今回は、テスト if(IsPost) が true を返すため、if 内のコード ブロックが実行されます。 コードは、メッセージ変数の値をマークアップでレンダリングされるコンテンツは、別の値に変更します。

今すぐ追加マークアップでの条件。 下、 &lt;p&gt;要素を含む、**送信** ボタンを次のマークアップを追加します。

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

開始する必要があるため、マークアップ内のコードを追加する@します。 If で追加した前コード ブロックのいずれかのようなテスト。 中かっこ内で、追加する通常の HTML などは通常取得するまで、少なくとも@DateTime.Nowします。 これは、別のほんの少しの Razor コードでためもう一度追加する必要が前にします。

ここでは、両方の条件コード ブロックおよびマークアップの上部にある場合を追加できることです。 If を使用する場合、ブロック内で行、ページの本文での条件には、マークアップまたはコードを指定できます。 その場合は、を使用して、コードが ASP.NET オフにすると、する必要があるときにいつでもマークアップとコードを混在させることが true の場合と。

ページを実行し、をクリックして**送信**します。 この時間だけでなくメッセージが表示別 ("これで登録した...") を送信するが、日付と時刻を一覧表示する新しいメッセージを参照してください。

!['Razor 2 test' ページの後を示すタイムスタンプで実行されているブラウザーの送信します。](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>クエリ文字列の値のテスト

1 つ以上のテスト。 今回は、if を追加します値をテストするブロックという番組をクエリ文字列で渡すことができます。 (このような: `http://localhost:43097/TestRazorPart2.cshtml?show=true`)、メッセージしたされてを表示するページを変更し ("This is 初めて..."など) は、表示の値が true の場合のみ表示されます。

下部にある (ただし、内側) で、次の追加、ページの上部にあるコード ブロック。

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

次の例のように完全なコード ブロックここで検索します。 (こと、ページに、コードをコピーする場合、インデント表示が異なるに注意してください。 ただし、コードの実行するには影響しません。)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

ブロック内の新しいコードでは、false に showMessage という名前の変数を初期化します。 場合は、テスト、クエリ文字列内の値を検索します。 最初に、ページを要求するときに次のような URL があります。

`http://localhost:43097/TestRazorPart2.cshtml`

コードでは、URL に、URL のこのバージョンのように、クエリ文字列の表示をという名前の変数が含まれるかどうかを決定します。

`http://localhost:43097/TestRazorPart2.cshtml`?show=true

テスト自体は、要求オブジェクトの QueryString プロパティを検索します。 True の場合にその項目が設定されている場合、クエリ文字列には、という名前の項目の表示が含まれている場合とブロックが実行され、showMessage 変数を true に設定します。

ここでは、トリックがあるご覧のとおりです。 名前が言うように、クエリ文字列、文字列です。 ただし、のみをテストできます true と false の場合、テストする値は、ブール値 (true または false) 値。 Show 変数の値をテストするには、クエリ文字列で、前に、ブール値に変換する必要があります。 AsBool メソッドを行っています — は入力として文字列を受け取り、ブール値に変換します。 明らかに、文字列が"true"の場合は、AsBool メソッドはその値を true に変換します。 文字列の値が何である場合、AsBool は false を返します。

> [!TIP] 
> 
> **データ型および As() メソッド**
> 
> のみが付いているこれまでに、変数を作成するときに、キーワード var 関数を使用すること ストーリー全体が。 値を操作するために、番号を追加または、文字列の連結や、日付を比較または true または false のテスト-c# 値の適切な内部表現を使用する必要があります。 C# できます*通常*表現である必要があります (は、どのような*型*データが) 値を使用して何をしているに基づいて。 行うことはできませんをします。 有効でない場合は、c# がデータを表す方法を明示的に指定して支援する必要があります。 AsBool メソッドでは、:"true"または"false"の文字列値をブール値として扱うこと (C#) に通知します。 AsInt (整数値として扱う)、AsDateTime (日付/時刻として扱う)、AsFloat (浮動小数点数として扱う) など、同様に、その他の種類として文字列を表す同様のメソッドが存在します。 () メソッドとしてこれらを使用して c# 表現できない文字列値の要求の場合、エラーが表示されます。


ページのマークアップを削除するか、この要素をコメント (ここに示されていますコメント アウトされた)。

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

右側の削除またはコメント アウト、そのテキストの位置は、次を追加します。

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

テストが showMessage 変数が true の場合を表示する場合は、 &lt;p&gt;メッセージ変数の値を持つ要素。

### <a name="summary-of-your-conditional-logic"></a>条件付きロジックの概要

のみ実行したのか、完全わからない場合は、ここで概要を示します。

- メッセージ変数は、既定の文字列に初期化されます (「これは、初めて...」)。
- "..."登録したのでにメッセージの値を変更するページ要求が送信 (post) の結果である場合は、
- ShowMessage 変数は、false に初期化されます。
- クエリ文字列が含まれている場合ですか? 表示 = showMessage 変数の設定が true の場合、true にします。
- ShowMessage が true の場合は、マークアップで、 &lt;p&gt;メッセージの値を示す要素が表示されます。 (ShowMessage が false の場合は、何もレンダリングその時点で、マークアップでされます。)
- 要求が、投稿の場合は、マークアップで、 &lt;p&gt;日付と時刻を表示する要素が表示されます。

ページを実行します。 If(showMessage) テストが、マークアップで false を返すために、showMessage が false の場合があるために、メッセージはありません。

**[送信]** をクリックします。 表示日付と時間が、メッセージはまだありません。

ブラウザーで URL ボックスに移動し、次の URL の末尾に追加:? 表示 = true とし、Enter キーを押します。

![クエリ文字列を表示するブラウザーでページの 'テスト Razor 2'](intro-to-web-pages-programming/_static/image5.png)

ページが再び表示されます。 (URL を変更したため、これは新しい要求を送信できません) です。クリックして**送信**もう一度です。 日付と時刻には、ここでも、メッセージが表示されます。

!['Razor 2 のテスト' ページの後にクエリ文字列がある場合の送信します。](intro-to-web-pages-programming/_static/image6.png)

URL に変更しますか? 表示 = true をでしょうか。 を表示する = false と Enter キーを押します。 ページをもう一度送信します。 ページが開始する方法には、メッセージはありません。

前述のように、この例のロジックは多少不自然です。 ただし、ページの多くで発生する場合は、したり、ここを見てきましたフォームの 1 つ以上がかかるされます。

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>ヘルパー (Gravatar イメージを表示する) をインストールします。

多くの場合、ユーザーは web ページで実行するいくつかのタスクは、多くのコードが必要または余分な知識が必要です。 例: データ用のグラフを表示します。ページで、Facebook「いいね!」ボタンを配置します。web サイトからメールを送信トリミングまたはのイメージをサイズ変更サイトの PayPal を使用します。 簡単にこれらの種類の項目を実行する ASP.NET Web Pages を使用できます*ヘルパー*します。 ヘルパーは、コンポーネントのサイトをインストールしてすれば、わずか数行の Razor コードを使用して一般的なタスクを実行することができます。

ASP.NET Web ページが、いくつかのヘルパーが組み込まれています。 ただし、多くのヘルパーは、NuGet パッケージ マネージャーを使用して提供されているパッケージ (アドインの場合) で利用できます。 インストールの詳細をすべて処理しと NuGet を使用してをインストールするパッケージを選択できます。

このチュートリアルでは、Gravatar (「グローバルに認識されたアバター」) のイメージを表示することができますヘルパーをインストールします。 次の 2 つをについて説明します。 検索して、ヘルパーをインストールする方法は 1 つです。 どのヘルパーを簡単に何か、多くのコードを自分で記述する必要がありますを使用して行う必要があるとそれ以外の場合も学習します。

Gravatar web サイトで、独自の Gravatar を登録する[ http://www.gravatar.com/](http://www.gravatar.com/)が、チュートリアルのこの部分を実行する Gravatar アカウントを作成する必要はありません。

WebMatrix で、をクリックして、 **NuGet**ボタンをクリックします。

![WebMatrix での NuGet ギャラリー ダイアログ ボックス](intro-to-web-pages-programming/_static/image7.png)

これにより、NuGet パッケージ マネージャーを起動し、利用可能なパッケージを表示します。 (ヘルパーはすべてのパッケージは、いくつかの追加機能自体 WebMatrix を一部は追加のテンプレートでありなど)。バージョンの非互換性についてエラー メッセージが表示することがあります。 クリックしてこのエラー メッセージは無視できます**OK**をこのチュートリアルを続行するとします。

![WebMatrix での NuGet ギャラリー ダイアログ ボックス](intro-to-web-pages-programming/_static/image8.png)

検索ボックスに、「asp.net ヘルパー」を入力します。 NuGet は、検索条件に一致するパッケージを示しています。

![パッケージを示す WebMatrix での NuGet ギャラリー](intro-to-web-pages-programming/_static/image9.png)

ASP.NET の Web Helpers Library には、Gravatar イメージの使用など、多くの一般的なタスクを簡略化するためのコードが含まれています。 選択、 **ASP.NET Web Helpers Library**パッケージ化し、をクリックし、**インストール**インストーラーを起動します。 選択**はい**されたら、パッケージをインストールし、インストールを完了する条項に同意するかどうか。

それで完了です。 NuGet をダウンロードしてインストールを必要となる可能性のあるコンポーネントを含めすべて (*依存関係*)。

何らかの理由は、ヘルパーをアンインストールする必要がある、プロセスはよく似ています。 をクリックして、 **NuGet**ボタンをクリックし、**インストール済み**タブをクリックしをアンインストールするパッケージを選択します。

## <a name="using-a-helper-in-a-page"></a>ページで、ヘルパーの使用

今すぐインストールしたヘルパーを使用します。 ページに、ヘルパーを追加するプロセスは、ほとんどのヘルパーと似ています。

WebMatrix で、ページを作成し、名前を付けます*GravatarTest.cshml*します。 (、ヘルパーをテストする特別なページを作成しているが、サイト内の任意のページでヘルパーを使用することができます)。

内で、&lt;本文&gt;要素を追加、 &lt;div&gt;要素。 内で、 &lt;div&gt;要素のように入力します。

@Gravatar。

Razor コードのマークを使用した同じ文字は、@ 文字。 **Gravatar**ヘルパー オブジェクトを扱う場合です。

WebMatrix の一覧を表示、ピリオド (.) を入力するとすぐに*メソッド*Gravatar ヘルパーが使用できるようにする (関数)。

![Gravatar ヘルパー IntelliSense ドロップダウン リスト](intro-to-web-pages-programming/_static/image10.png)

この機能と呼ばれる*IntelliSense*します。 コンテキストに適した選択肢を提供することで、コードを支援します。 IntelliSense は、HTML、CSS、ASP.NET コード、JavaScript、および WebMatrix でサポートされているその他の言語で動作します。 もう 1 つの機能を WebMatrix で web ページを開発するが容易になります。

G キーを押すと、キーボードでは、IntelliSense が GetHtml メソッドを検出できるを参照してください。 Tab キーを押します。IntelliSense を選択したメソッド (GetHtml) を挿入します。 開きかっこを入力し、閉じかっこが自動的に追加されたことに注意してください。 引用符を 2 つのかっこの間で、電子メール アドレスを入力します。 Gravatar アカウントがある場合は、プロファイル写真が返されます。 Gravatar アカウントがない、既定のイメージが返されます。 完了すると、行はようになります。

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

今すぐ、ブラウザーでページを表示します。 Gravatar アカウントを持っているかどうかによって、画像、または既定のイメージのいずれかに表示されます。

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![既定の画像](intro-to-web-pages-programming/_static/image12.png)

内容を把握する、ヘルパーを実行、ブラウザーでページのソースを表示します。 HTML ページにする必要があるが、と共に識別子を含むイメージ要素を参照してください。 これは、ヘルパーが必要があった場所にページにレンダリング コード@Gravatar.GetHtmlします。 ヘルパーは、情報を提供して Gravatar に直接では説明の指定したアカウントの適切なイメージを取得するためにコードを生成しました。

GetHtml メソッドでは、その他のパラメーターを提供することで、イメージをカスタマイズすることもできます。 次のコードは、イメージの幅と高さが 40 ピクセル、およびという名前を指定した既定のイメージを使用して要求する方法を示します**wavatar**指定されたアカウントが存在しない場合。

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

このコードでは、(既定のイメージは異なるランダム) 結果は次のようなものが生成されます。

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>次回について

このチュートリアルの短い形式を保持するには、は、いくつかの基本のみに注目する必要があります。 当然ながらは、*多く*Razor および c# の詳細。 これらのチュートリアルを経由するよりも多くについて説明します。 Razor のプログラミングの側面の詳細に関心がある場合とC#今ここに詳しい概要を読み取ることができます。[Razor 構文を使用して ASP.NET Web プログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=202890)します。

次のチュートリアルでは、データベースの操作を紹介します。 そのチュートリアルでは、お気に入りの映画を一覧表示することができます、サンプル アプリケーションの作成を開始します。

## <a name="complete-listing-for-testrazor-page"></a>TestRazor ページ全体を示します

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>TestRazorPart2 ページ全体を示します

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>GravatarTest ページ全体を示します

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>その他のリソース

- [Razor 構文を使用して ASP.NET Web プログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter ヘルパー](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [前へ](getting-started.md)
> [次へ](displaying-data.md)