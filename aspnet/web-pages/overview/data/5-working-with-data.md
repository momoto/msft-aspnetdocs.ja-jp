---
uid: web-pages/overview/data/5-working-with-data
title: ページ (Razor) サイトを ASP.NET Web でのデータベース操作の概要 |Microsoft Docs
author: Rick-Anderson
description: この章では、データベースからデータにアクセスし、ASP.NET Web Pages を使用して表示する方法について説明します。
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 45e988d037465e59ad352bb9444af2c69fd3cd70
ms.sourcegitcommit: dd0dc556a3d99a31d8fdbc763e9a2e53f3441b70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/27/2019
ms.locfileid: "67411265"
---
# <a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>ページ (Razor) サイトを ASP.NET Web でのデータベース操作の概要

によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、Microsoft WebMatrix ツールを使用して、ASP.NET Web Pages (Razor) の web サイトでデータベースを作成する方法と表示、追加、編集、およびデータを削除することができるページを作成する方法について説明します。
> 
> **学習内容。** 
> 
> - データベースを作成する方法。
> - データベースに接続する方法。
> - Web ページにデータを表示する方法。
> - 挿入、更新、およびデータベースのレコードを削除する方法。
> 
> この記事で導入された機能を次に示します。
> 
> - Microsoft SQL Server Compact Edition データベースを使用します。
> - SQL クエリを使用します。
> - `Database` クラス
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> このチュートリアルは、WebMatrix 3 でも機能します。 ASP.NET Web ページ 3 と Visual Studio 2013 (または Visual Studio Express 2013 for Web) を使用することができます。ただし、ユーザー インターフェイスは別になります。

## <a name="introduction-to-databases"></a>データベースの概要

通常、アドレス帳を想像してください。 アドレス帳のエントリごとに (つまり、各人の) 名、姓、住所、電子メール アドレス、および電話番号などのいくつかの情報があります。

このような画像データへの一般的な方法は、行と列を含むテーブルとしてです。 データベース用語では、各行は多くの場合にレコードとして参照します。 (フィールドとも呼ばれます) の各列には、データの種類ごとの値が含まれています。 名、姓の名、およびなど。

| **ID** | **FirstName** | **LastName** | **Address** | **電子メール** | **電話** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main st. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

ほとんどのデータベース テーブル、テーブルは、顧客番号、口座番号などのように、一意の識別子を含む列がある必要があります。これは、テーブルのと呼ばれます*主キー*テーブル内の各行を識別するために使用するとします。 例では、ID 列は、アドレス帳の主キーです。

データベースの基本的な理解するおくと、単純なデータベースを作成し、追加、変更、およびデータの削除などの操作を実行する方法を学習する準備ができました。

> [!TIP] 
> 
> **リレーショナル データベース**
> 
> 多くの方法があり、テキスト ファイル、およびスプレッドシートでデータを格納することができます。 ほとんどのビジネス用途のため、データはリレーショナル データベースでします。
> 
> この記事では、データベースに非常に密接に移動しません。 ただし、役立つことについて少し理解します。 リレーショナル データベースでは、別々 のテーブルに情報が論理的に分割されます。 たとえば、学校のデータベースは、受講者とクラスのソリューションの別個のテーブルを含む可能性があります。 データベース (SQL Server) などのソフトウェア サポート強力なコマンドを動的には、テーブル間のリレーションシップを確立します。 たとえば、スケジュールを作成するには、生徒やクラス間に論理リレーションシップを確立するために、リレーショナル データベースを使用することができます。 別のテーブルにデータを格納するテーブル構造の複雑さを軽減し、冗長なデータをテーブルに保持する必要性を軽減します。

## <a name="creating-a-database"></a>データベースの作成

この手順では、WebMatrix に含まれている SQL Server Compact データベース設計ツールを使用して SmallBakery をという名前のデータベースを作成する方法を示します。 コードを使用してデータベースを作成できますが、データベースと WebMatrix のようなデザイン ツールを使用してデータベース テーブルを作成するより一般的です。

1. WebMatrix を起動し、クイック スタート ページで、**テンプレートからサイト**します。
2. 選択**空のサイト**、し、**サイト名**ボックスは、"SmallBakery"を入力し、クリックして **[ok]** します。 サイトが作成され、WebMatrix で表示されます。
3. 左側のウィンドウでをクリックして、**データベース**ワークスペース。
4. リボンで、次のようにクリックします。**新しいデータベース**します。 サイトと同じ名前の空のデータベースが作成されます。
5. 左側のウィンドウで、展開、 **SmallBakery.sdf**ノードをクリック**テーブル**します。
6. リボンで、次のようにクリックします。**新しいテーブル**します。 WebMatrix では、テーブル デザイナーが開きます。

    ![[イメージ]](5-working-with-data/_static/image1.jpg)
7. をクリックして、**名前**列を入力し、 &quot;Id&quot;します。
8. **データ型**列で、 **int**します。
9. 設定、**プライマリ キーであるか?** と**を識別するでしょうか。** オプションを**はい**。

    名前からわかるように、**プライマリ キーである**設定により、データベース テーブルの主キーなります。 **[Is Identity]** 設定により、データベース (1 から始まります)、次の連続番号を割り当てると、すべての新しいレコードの ID 番号を自動的に作成します。
10. 次の行をクリックします。 エディターでは、新しい列定義を開始します。
11. 名前の値を入力&quot;名前&quot;します。
12. **データ型**、選択&quot;nvarchar&quot;と長さを 50 に設定します。 *Var*一部分`nvarchar`データベースにこの列のデータ サイズは、レコード間を異なる場合があります文字列になることを指示します。 (、 *N*プレフィックスの表す*national*フィールドがすべてアルファベットを表す文字データを保持できることを示す、または、書記&#8212;は、フィールドが Unicode データを保持している)。
13. 設定、 **Null を許容**オプションを**いいえ**します。 これは、テンプレートを適用する、*名前*列がない空白のままにします。
14. という名前の列を作成するこの同じプロセスを使用して*説明*します。 設定**データ型**"nvarchar"の長さ、およびセット 50 を**Null を許容**を false にします。
15. という名前の列を作成する*価格*します。 設定 **"money"データ型**設定と**Null を許容**を false にします。
16. 上部にあるボックスで、テーブルの名前を&quot;製品&quot;します。

    完了すると、定義は次のようになります。

    ![[イメージ]](5-working-with-data/_static/image2.png)
17. テーブルを保存するには、Ctrl + S キーを押します。

## <a name="adding-data-to-the-database"></a>データベースへのデータの追加

これで、記事の後半で使用するのデータベースにいくつかのサンプル データを追加できます。

1. 左側のウィンドウで、展開、 **SmallBakery.sdf**ノードをクリック**テーブル**します。
2. Product テーブルを右クリックし、をクリックし、**データ**します。
3. [編集] ペインで、次のレコードを入力します。

    | **Name** | **説明** | **価格** |
    | --- | --- | --- |
    | パン | 毎日斬新組み込まれています。 | 2.99 |
    | ストロベリー ショート ケーキ | ガーデンから有機的な strawberries で行われます。 | 9.99 |
    | Apple の円 | Mom の円グラフにのみ 2 番目です。 | 12.99 |
    | Pecan 円 | Pecans 場合は、これは。 | 10.99 |
    | レモンの円 | 世界中で最適な lemons で行われます。 | 11.99 |
    | Cupcakes | 自分の子供とで kid は、これらでが大好きです。 | 7.99 |

    何も入力する必要はないことに注意してください、 *Id*列。 作成したときに、 *Id*列で、設定、 **Id**プロパティに自動的に入力するには、true をします。

    データの入力が完了したら、テーブル デザイナーは次のようになります。

    ![[イメージ]](5-working-with-data/_static/image3.jpg)
4. データベースのデータを含むタブを閉じます。

## <a name="displaying-data-from-a-database"></a>データベースからデータを表示します。

これで、データベースにデータをした後は、ASP.NET web ページで、データを表示できます。 表示するテーブルの行を選択するには、データベースに渡すコマンドは、SQL ステートメントを使用します。

1. 左側のウィンドウでをクリックして、**ファイル**ワークスペース。
2. Web サイトのルートに作成するという名前の新しい CSHTML ページ*ListProducts.cshtml*します。
3. 次のように既存のマークアップに置き換えます。

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    最初のコード ブロックで開く、 *SmallBakery.sdf*先ほど作成したファイル (データベース)。 `Database.Open`メソッドを前提としていますが、 *.sdf*ファイルは、web サイトの*アプリ\_データ*フォルダー。 (を指定する必要はありませんが、 *.sdf*拡張子&#8212;実のところ、この場合、`Open`メソッドは機能しません)。

    > [!NOTE]
    > *アプリ\_データ*データ ファイルを格納するために使用する asp.net の特別なフォルダーです。 詳細については、次を参照してください。[データベースに接続する](#SB_ConnectingToADatabase)この記事で後述します。

    次の SQL を使用してデータベースを照会する要求を行うし`Select`ステートメント。

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    ステートメントでは、`Product`クエリにテーブルを識別します。 `*`文字は、クエリがテーブルからすべての列を返す必要がありますを指定します。 (でしたもを一覧表示の列に個別に、一部の列のみを表示する場合は、コンマで区切っています。)`Order By`句では、データの並べ替え方法を示します&#8212;ここでは、によって、*名前*列。 これは、データの値に基づいてアルファベット順にソートされていることを意味、*名前*各行の列。

    ページの本文には、マークアップは、データを表示するために使用される HTML テーブルを作成します。 内で、`<tbody>`要素を使用する、`foreach`ループを使用して、クエリによって返されるデータ行ごとに個別に取得します。 HTML テーブルの行を作成する各データ行 (`<tr>`要素)。 HTML テーブルのセルを作成し、(`<td>`要素) の各列。 ループでアクセスするたび、データベースから、[次へ] の使用可能な行が、`row`変数 (に設定する、`foreach`ステートメント)。 使用することができます、個々 の列行からを取得する`row.Name`または`row.Description`またはする列は、任意の名前。
4. ブラウザーでページを実行します。 (内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。ページには、次のような一覧が表示されます。

    ![[イメージ]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **構造化照会言語 (SQL)**
> 
> SQL は、データベース内のデータを管理するために、ほとんどのリレーショナル データベースで使用される言語です。 これには、データを取得し、それを更新できるようにして、作成、変更、およびデータベースのテーブルを管理するためのコマンドが含まれます。 SQL が (webmatrix を使用している 1 つ) などのプログラミング言語と異なるための考え方は、データベースを指示すること、データベースのジョブ、データの取得や、タスクを実行する方法を把握するは SQL とします。 一部の SQL コマンドの例とその実行内容を次に示します。
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> これをフェッチ、 *Id*、*名前*、および*価格*列内のレコードから、*製品*場合のテーブルの値*価格* 10 以上の値に基づいてアルファベット順で結果を返す、*名前*列。 このコマンドでは、レコードが一致しない場合、条件、または空のセットに一致するレコードを含む結果セットを返します。
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> これに新しいレコードが挿入、*製品*テーブル、設定、*名前*列を&quot;クロワッサン&quot;、*説明*列&quot;不安定な満足&quot;とから 1.99 する価格。
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> このコマンドのレコードの削除、*製品*が 2008 年 1 月 1 日より前で有効期限の日付列が含まれるテーブル。 (これで、*製品*テーブルにはこのような列は、もちろん)。/MM/DD/YYYY の形式で日付がここに入力したが、現在のロケールに使用される形式で入力してください。
> 
> `Insert Into`と`Delete`コマンドの結果セットを返しません。 代わりに、コマンドの影響を受けたレコードの数を示す数値が返されます。
> 
> これらの操作 (挿入や、レコードを削除する) などの一部は、操作を要求しているプロセスは、データベース内に適切なアクセス許可があります。 これは、実稼働データベースの多くの場合、データベースに接続するときに、ユーザー名とパスワードを指定する必要がある理由です。
> 
> 数十個の SQL コマンドがありますが、このようなパターンに従うすべて。 SQL コマンドを使用して、データベース テーブルを作成、テーブル内のレコードの数をカウント、価格、計算、および多くの操作を実行することができます。

## <a name="inserting-data-in-a-database"></a>データベースでデータを挿入

このセクションでは、ユーザーが新しい製品を追加できるページを作成する方法を示しています、*製品*データベース テーブル。 更新されたテーブルを使用して、新しい製品レコードが挿入されると、ページに表示されます、 *ListProducts.cshtml*前のセクションで作成したページです。

ページには、ユーザーが入力したデータが、データベースに対して有効であるかどうかを確認する検証が含まれます。 たとえば、ページ内のコードは、必要なすべての列の値が入力されていることを確認します。

1. という名前の新しい CSHTML ファイルを作成、web サイトで*InsertProducts.cshtml*します。
2. 次のように既存のマークアップに置き換えます。

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    ページの本文には、ユーザー名、説明、および価格を入力できるようにする 3 つのテキスト ボックスに HTML フォームが含まれています。 ユーザーがクリックすると、**挿入**ボタン、ページの上部にあるコードへの接続が表示されます、 *SmallBakery.sdf*データベース。 使用して、ユーザーが送信される値を取得、`Request`オブジェクトし、それらの値をローカル変数に割り当てます。

    各ユーザーが必要な列の値を入力することを検証するには、登録する`<input>`検証する要素。

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation`ヘルパーは、各登録したフィールドの値があることを確認します。 すべてのフィールドを確認して検証に合格するかどうかをテストする`Validation.IsValid()`、通常は、ユーザーから取得した情報を処理する前に。

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (、`&&`演算子は AND — このテストが*フォームの送信接続が、すべてのフィールドが検証に合格*)。

    すべての列を検証する場合 (なかった空)、データを挿入し、次に示すように実行する SQL ステートメントを作成してください。

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    値を挿入するには、パラメーターのプレース ホルダーが含まれます。 (`@0`、 `@1`、 `@2`)。

    > [!NOTE]
    > セキュリティの予防措置として常に値を渡すパラメーターを使用して SQL ステートメントにように前の例を参照してください。 これにより、ユーザーのデータを検証することと、データベース (SQL インジェクション攻撃とも呼ばれます) に悪意のあるコマンドの送信を防ぐのに役立ちます。

    クエリを実行するには、ステートメントを使用するこのプレース ホルダーの代わりに値を格納する変数を渡します。

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    後に、`Insert Into`ステートメントが実行される、この行を使用して製品を一覧表示されたページにユーザーを送信します。

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    検証に成功しなかった場合は、挿入をスキップします。 代わりに、蓄積されたエラー メッセージ (あれば) を表示できるページに、ヘルパーがあります。

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    マークアップにスタイル ブロックがという名前の CSS クラス定義が含まれることに注意してください`.validation-summary-errors`します。 これは、既定で使用される CSS クラスの名前、`<div>`検証エラーを含む要素。 この場合は、CSS クラスすることを指定検証概要エラーが赤で表示され、太字で、定義することができます、`.validation-summary-errors`たい書式を表示するクラス。

### <a name="testing-the-insert-page"></a>Insert ページのテスト

1. ブラウザーでページを表示します。 次の図に示すようなフォームを表示します。

    ![[イメージ]](5-working-with-data/_static/image5.jpg)
2. すべての列の値を入力しますが、しておくことを確認、*価格*列は空白です。
3. **[挿入]** をクリックします。 ページでは、次の図に示すように、エラー メッセージが表示されます。 (新しいレコードは作成されません。)

    ![[イメージ]](5-working-with-data/_static/image6.jpg)
4. フォームに完全に入力し、**挿入**します。 今回は、 *ListProducts.cshtml*ページが表示され、新しいレコードを示しています。

## <a name="updating-data-in-a-database"></a>データベース内のデータを更新しています

テーブルにデータを入力すると、更新する必要があります。 この手順では、データの挿入前に作成したものに類似した 2 つのページを作成する方法を示します。 最初のページでは、製品を表示し、ユーザーを変更する 1 つを選択します。 2 番目のページには、実際には、編集し、保存、ユーザーことができます。

> [!NOTE] 
> 
> **重要な**を運用環境の web サイトを通常できるユーザーを制限、データを変更します。 サイトのタスクを実行するユーザーを承認する方法についてとメンバーシップを設定する方法については、次を参照してください。[追加のセキュリティと ASP.NET Web ページ サイトには、メンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904)します。

1. という名前の新しい CSHTML ファイルを作成、web サイトで*EditProducts.cshtml*します。
2. 次のように、ファイルの既存のマークアップに置き換えます。

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    このページの唯一の違い、 *ListProducts.cshtml*ページから前は、このページの HTML テーブルに表示する追加の列が含まれている、**編集**リンク。 かかるこのリンクをクリックすると、 *UpdateProducts.cshtml*ページ (次に作成します) が選択されているレコードを編集することができます。

    作成するコードを見て、**編集**リンク。

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    これは、HTML を作成します。`<a>`要素が`href`属性が動的に設定します。 `href`属性は、ユーザーがリンクをクリックしたときに表示するページを指定します。 また渡します、`Id`のリンクを現在の行の値。 ページを実行すると、ページのソースには、上記のようなリンクが含まれます。

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    注意、`href`属性に設定されて`UpdateProducts/n`ここで、 *n*は製品の数です。 ユーザーには、これらのリンクをクリックすると、結果の URL は次のようになります。

    `http://localhost:18816/UpdateProducts/6`

    つまり、製品番号の編集は、URL で渡されます。
3. ブラウザーでページを表示します。 ページには、このような形式で、データが表示されます。

    ![[イメージ]](5-working-with-data/_static/image7.jpg)

    次に、ユーザーが実際には、データを更新できるページを作成します。 更新プログラム ページには、ユーザーが入力したデータを検証する検証が含まれています。 たとえば、ページ内のコードは、必要なすべての列の値が入力されていることを確認します。
4. という名前の新しい CSHTML ファイルを作成、web サイトで*UpdateProducts.cshtml*します。
5. 次のように、ファイルの既存のマークアップを置き換えます。

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    ページの本文には、製品が表示され、ユーザーが編集できる HTML フォームが含まれています。 表示する製品を取得するには、この SQL ステートメントを使用します。

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    これにより、ID が渡される値に一致する製品を選択、`@0`パラメーター。 (ため*Id*は主キーとそのため必要がありますで一意である、1 つの製品レコードがこの方法を選択するこれまでことができます)。これに渡す ID 値を取得する`Select`ステートメントに渡されるページの URL の一部として、次の構文を使用して値を読み取ることができます。

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    製品レコードを実際にフェッチするを使用する、`QuerySingle`メソッドは、1 つのレコードが返されます。

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    1 つの行が返されます、`row`変数。 各列からデータを取得し、次のようにローカル変数に割り当てることができます。

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    フォームのマークアップでは、これらの値は、次のような埋め込みコードを使用して、個々 のテキスト ボックスで自動的に表示します。

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    コードの部分には、更新する製品レコードが表示されます。 レコードが表示された後、ユーザーは、個々 の列を編集できます。

    クリックして、ユーザーがフォームを送信するときに、**更新**コードでは、ボタン、`if(IsPost)`ブロックが実行されます。 これから、ユーザーの値を取得、`Request`オブジェクトは、変数値を格納し各列が入力されていることを検証します。 検証に合格した場合、コードは、次の SQL の Update ステートメントを作成します。

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    SQL で`Update`ステートメントに設定する値で更新するには、各列を指定します。 このコードで、値はパラメーターのプレース ホルダーを使用して`@0`、 `@1`、`@2`など。 (セキュリティのため、前述したようを渡す必要があります常に値を SQL ステートメントにパラメーターを使用しています。)

    呼び出すと、`db.Execute`メソッドでは、SQL ステートメント内のパラメーターに対応する順序で値を格納する変数を渡します。

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    後に、`Update`ステートメントが実行された、ユーザーを編集 ページにリダイレクトするには、次のメソッドを呼び出します。

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    効果は、ユーザーがデータベース内のデータの更新の一覧が表示し、別の製品を編集することができます。
6. ページを保存します。
7. 実行、 *EditProducts.cshtml*ページ (更新プログラム ページ) をクリック**編集**を編集する製品を選択します。 *UpdateProducts.cshtml*ページを表示すると、選択したレコードを表示します。

    ![[イメージ]](5-working-with-data/_static/image8.jpg)
8. 変更を加えるし、クリックして**Update**します。 製品一覧が更新されたデータをもう一度表示されます。

## <a name="deleting-data-in-a-database"></a>データベース内のデータを削除します。

このセクションではユーザーからの製品を削除できるようにする方法、*製品*データベース テーブル。 この例は、2 つのページで構成されます。 最初のページでは、ユーザーは、削除するレコードを選択します。 レコードを削除するレコードを削除することを確認することができます、2 番目のページに表示されます。

> [!NOTE] 
> 
> **重要な**を運用環境の web サイトを通常できるユーザーを制限、データを変更します。 サイトのタスクを実行するユーザーを承認する方法についてとメンバーシップを設定する方法については、次を参照してください。[追加のセキュリティと ASP.NET Web ページ サイトには、メンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904)します。

1. という名前の新しい CSHTML ファイルを作成、web サイトで*ListProductsForDelete.cshtml*します。
2. 次のように既存のマークアップに置き換えます。

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    このページと似ています、 *EditProducts.cshtml*前のページ。 表示する代わりに、ただし、**編集**リンク、各製品が表示されます、**削除**リンク。 **削除**マークアップに埋め込まれた次のコードを使用してリンクを作成します。

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    これには、ユーザーがリンクをクリックすると次のような URL が作成されます。

    `http://<server>/DeleteProduct/4`

    URL がという名前のページを呼び出す*DeleteProduct.cshtml* (これは、次に作成します) を削除する、製品の ID を渡します (ここで、4)。
3. ファイルを保存しますが、開いたままにします。
4. という名前の別の CHTML ファイル作成*DeleteProduct.cshtml*します。 次のように、既存のコンテンツを置き換えます。

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    このページは、によって呼び出されます。 *ListProductsForDelete.cshtml*でき、ユーザーが製品を削除することを確認します。 削除する製品を一覧表示するには、ID を入手する、製品の URL から削除する次のコードを使用します。

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    ページには、実際にレコードを削除するボタンをクリックするユーザーが求められます。 これは、重要なセキュリティ対策: これらの操作を行う常に更新またはデータを削除するように、web サイトで機密情報の操作を実行すると POST 操作で GET 操作ではありません。 GET 操作を使用して削除操作を実行できるように、サイトを設定する、すべてのユーザーとのような URL を渡すことが`http://<server>/DeleteProduct/4`をデータベースから削除する必要なものとします。 確認を追加して、削除は、POST を使用してのみ実行できるように、ページのコーディングで、サイト セキュリティのメジャーを追加します。

    実際の削除操作は、まず post 操作であるし、ID が空でないことを確認する次のコードを使用して実行されます。

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    コードでは、指定されたレコードを削除し、一覧ページに戻るユーザーをリダイレクトする SQL ステートメントを実行します。
5. 実行*ListProductsForDelete.cshtml*ブラウザーでします。

    ![[イメージ]](5-working-with-data/_static/image9.jpg)
6. をクリックして、**削除**製品のいずれかのリンク。 *DeleteProduct.cshtml*ページが表示され、そのレコードを削除することを確認します。
7. をクリックして、**削除**ボタンをクリックします。 製品レコードが削除され、製品の更新の一覧で、ページが更新されます。

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>データベースへの接続
> 
> 2 つの方法でデータベースに接続することができます。 1 つ目は、使用する、`Database.Open`メソッドと、データベース ファイルの名前を指定する (以下、 *.sdf*拡張機能)。
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open`メソッドには、ことが前提とします *。sdf*ファイルは、web サイトの*アプリ\_データ*フォルダー。 このフォルダーは、データを保持する向けに設計されています。 たとえばに必要なデータ、読み取りし、書き込みを許可する適切なアクセス許可があるし、セキュリティ対策として WebMatrix もアクセスできないファイルをこのフォルダーから。
> 
> 2 番目の方法では、接続文字列を使用します。 接続文字列には、データベースに接続する方法に関する情報が含まれています。 これは、ファイルのパスを含めることができます、またはユーザー名とそのサーバーに接続するためのパスワードと共に、ローカルまたはリモート サーバー上の SQL Server データベースの名前を含めることができます。 (一元管理されたバージョンの SQL Server にデータを保持する場合など、ホスティング プロバイダーのサイトで常に使用する接続文字列をデータベース接続情報を指定する。)
> 
> WebMatrix で、接続文字列は、通常はという XML ファイルに格納*Web.config*します。使用することができます、名のとおり、 *Web.config*サイトを必要とする接続文字列など、サイトの構成情報を格納する web サイトのルートにあるファイル。 内の接続文字列の例を*Web.config*ファイルは、次のようになります。
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> 例では、接続文字列がどこかに、サーバーで実行されている SQL Server のインスタンスでデータベースを指す (ローカルではなく *.sdf*ファイル)。 適切な名前を置き換える必要があります`myServer`と`myDatabase`、SQL Server ログインの値を指定し、`username`と`password`します。 (ユーザー名とパスワードの値が必ずしも同じでなければ、Windows 資格情報として、または、サーバーへのログインに提供された、ホスティング プロバイダーの値として。 管理者に確認する必要がある正確な値にします。)
> 
> `Database.Open`メソッドは、データベースの名前を渡すことができるので柔軟性が高く、 *.sdf*ファイルまたはに格納されている接続文字列の名前、 *Web.config*ファイル。 次の例では、前の例に示す接続文字列を使用してデータベースに接続する方法を示します。
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> 前述のように、`Database.Open`メソッドでは、データベース名または接続文字列のいずれかを渡すことができ、それがどれを使用するを算出します。 これは、展開する場合に非常に便利です (発行)、web サイト。 使用することができます、 *.sdf*ファイル、*アプリ\_データ*フォルダーを開発して、サイトのテストをします。 内の接続文字列を使用するには、サイトを実稼働サーバーに移動すると、その後、 *Web.config*ファイルと同じ名前を持つ、 *.sdf* &#8212;せずに、コードを変更します。
> 
> 最後に、接続文字列を直接操作する場合を呼び出すことができます、`Database.OpenConnectionString`メソッドと実際の接続が文字列で 1 つの名だけでなく、パス、 *Web.config*ファイル。 何らかの理由はありませんがある場合、接続文字列へのアクセスに役立つことが考えられます (などの値、または、 *.sdf*ファイル名)、ページが実行されるまでです。 ただし、ほとんどのシナリオで使用できます`Database.Open`この記事で説明します。

## <a name="additional-resources"></a>その他のリソース

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [SQL Server または WebMatrix の MySQL データベースに接続します。](https://go.microsoft.com/fwlink/?LinkId=208661)
- [ASP.NET Web ページにおけるユーザー入力の検証](https://go.microsoft.com/fwlink/?LinkId=253002)
