---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'チュートリアル: ASP.NET MVC アプリ用により複雑なデータモデルを作成する'
author: tdykstra
description: このチュートリアルでは、エンティティとリレーションシップをさらに追加し、書式設定、検証、およびデータベースマッピング規則を指定して、データモデルをカスタマイズします。
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 933354b09eb4674e6f523f8a65816410521f026f
ms.sourcegitcommit: aa3c2efd56466fc6bdc387ee01ad6f50a261665b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/26/2019
ms.locfileid: "70020998"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>チュートリアル: ASP.NET MVC アプリ用により複雑なデータモデルを作成する

前のチュートリアルでは、3つのエンティティで構成された単純なデータモデルを使用しました。 このチュートリアルでは、エンティティとリレーションシップをさらに追加し、書式設定、検証、およびデータベースマッピング規則を指定して、データモデルをカスタマイズします。 この記事では、エンティティクラスに属性を追加し、データベースコンテキストクラスにコードを追加することによって、データモデルをカスタマイズする2つの方法について説明します。

完了すると、エンティティ クラスは、以下の図のように完成したデータ モデルを構成します。

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * データモデルをカスタマイズする
> * Student エンティティの更新
> * Instructor エンティティを作成する
> * OfficeAssignment エンティティを作成する
> * Course エンティティを変更する
> * Department エンティティを作成する
> * Enrollment エンティティを変更する
> * データベースコンテキストへのコードの追加
> * テスト データを使ってデータベースをシードする
> * 移行を追加する
> * データベースを更新する

## <a name="prerequisites"></a>必須コンポーネント

* [移行と展開の Code First](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>データモデルをカスタマイズする

このセクションでは、書式設定、検証、データベース マッピング規則を指定する属性を使用して、データ モデルをカスタマイズする方法を示します。 次のセクションでは、既に作成したクラス`School`に属性を追加し、モデル内の残りのエンティティ型に対して新しいクラスを作成することによって、完全なデータモデルを作成します。

### <a name="the-datatype-attribute"></a>DataType 属性

学生の登録日について、すべての Web ページでは現在、日付と共に時刻が表示されていますが、このフィールドでは日付が重要になります。 データ注釈属性を使用すれば、1 つのコードを変更するだけで、データを表示するすべてのビューの表示形式を修正できます。 その方法例を表示するには、`Student` クラスの `EnrollmentDate` プロパティに属性を追加します。

*Models\Student.cs*で、次の`using`例に示す`System.ComponentModel.DataAnnotations`ように、 `DataType` `EnrollmentDate`名前`DisplayFormat`空間のステートメントを追加し、プロパティに属性と属性を追加します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、データベースの組み込み型よりも具体的なデータ型を指定するために使用されます。 この例では、追跡する必要があるのは、日付と時刻ではなく、日付のみです。 [DataType 列挙型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)は、 *Date、Time、PhoneNumber、Currency、EmailAddress*など、多くのデータ型に対応しています。 また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。 たとえば、 [EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)に`mailto:`対してリンクを作成し、[データ型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)に対して日付セレクターを指定することができます。 [HTML5](http://html5.org/)をサポートするブラウザーで日付を指定できます。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、html 5 ブラウザーが理解できる html 5[データ](http://ejohn.org/blog/html-5-data-attributes/)("*データダッシュ*" と読みます) 属性を出力します。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は検証を提供しません。

`DataType.Date` は、表示される日付の書式を指定しません。 既定では、データフィールドは、サーバーの[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)に基づく既定の形式に従って表示されます。

`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

この`ApplyFormatInEditMode`設定では、編集のためにテキストボックスに値を表示するときに、指定した書式設定も適用する必要があることを指定します。 (たとえば、通貨値の場合、テキストボックスに通貨記号を編集する必要がないなどの一部のフィールドには必要ありません)。

[Displayformat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)属性は単独で使用できますが、通常は[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性も使用することをお勧めします。 属性`DataType`は、画面に表示する方法とは対照的に、データの*セマンティクス*を伝達し`DisplayFormat`ます。また、次のような利点があります。

- ブラウザーは HTML5 機能を有効にすることができます (たとえば、カレンダー コントロール、ロケールに適した通貨記号、メール リンク、一部のクライアント側の入力検証を表示するときなど)。
- 既定では、ブラウザーは[ロケール](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)に基づいて正しい形式を使用してデータを表示します。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性を使用すると、MVC でデータを表示するための適切フィールドテンプレートを選択できます ( [displayformat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)では文字列テンプレートが使用されます)。 詳細については、「Brad Wilson の[ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)」を参照してください。 (ただし、MVC 2 用に記述されていますが、この記事は現在のバージョンの ASP.NET MVC にも当てはまります)。

日付フィールドで`DataType`属性を使用する場合は、フィールドが Chrome ブラウザーで`DisplayFormat`正しく表示されるように属性も指定する必要があります。 詳細については、[この StackOverflow スレッド](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)を参照してください。

Mvc で他の日付形式を処理する方法の詳細について[は、mvc 5 の概要に関するページを参照してください。国際化](../introduction/examining-the-edit-methods-and-edit-view.md) &quot;のために、ページ内の edit メソッドと edit ビューを調べて検索します。&quot;

[学生用のインデックス] ページをもう一度実行すると、登録日の時刻が表示されなくなっていることがわかります。 `Student`モデルを使用するすべてのビューについても同じことが当てはまります。

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>Stringlength 属性

属性を使用して、データ検証規則と検証エラー メッセージを指定することもできます。 [Stringlength 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)は、データベースの最大長を設定し、ASP.NET MVC のクライアント側とサーバー側の検証を提供します。 この属性で最小長を指定することもできますが、最小値はデータベース スキーマに影響しません。

たとえば、ユーザーが 50 文字を超える名前を入力しないようにする必要があるとします。 この制限を追加するには、次の例`LastName`に`FirstMidName`示すように、プロパティとプロパティに[stringlength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性を追加します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[Stringlength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性は、ユーザーが名前に空白を入力するのを防ぐことはできません。 [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)属性を使用して、入力に制限を適用できます。 たとえば、次のコードでは、最初の文字を大文字にする必要があり、残りの文字はアルファベット順にする必要があります。

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)属性は[stringlength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性と同様の機能を提供しますが、クライアント側の検証は提供しません。

アプリケーションを実行し、 **[Students]** タブをクリックします。次のエラーが表示されます。

*データベースの作成後に、' Schoolcontext.cs ' コンテキストをバッキングするモデルが変更されました。Code First Migrations を使用してデータベースを更新[https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)することを検討してください ()。*

データベースモデルは、データベーススキーマの変更を必要とするように変更されているため、Entity Framework 検出されました。 移行を使用して、UI を使用してデータベースに追加したデータを失うことなく、スキーマを更新します。 `Seed`メソッドによって作成されたデータを変更した場合、 `Seed`メソッドで使用している[addorupdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)メソッドが原因で、そのデータが元の状態に戻されます。 ([Addorupdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)は、データベース用語からの "upsert" 操作に相当します)。

パッケージ マネージャー コンソール (PMC) で、次のコマンドを入力します。

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

この`add-migration`コマンドは、  *&lt;timeStamp&gt;MaxLengthOnNames.csという名前のファイルを\_* 作成します。 このファイルには、現在のデータ モデルと一致するようにデータベースを更新する `Up` メソッドのコードが含まれます。 `update-database` コマンドでそのコードが実行されました。

移行ファイル名の前に付加されたタイムスタンプは、移行の順序を指定するために Entity Framework によって使用されます。 `update-database`コマンドを実行する前に複数の移行を作成した後、すべての移行が作成された順序で適用されます。

**[作成]** ページを実行し、50文字を超える名前を入力します。 **[作成]** をクリックすると、クライアント側の検証でエラーメッセージが表示されます。*LastName フィールドは、最大長が50の文字列である必要があります。*

### <a name="the-column-attribute"></a>Column 属性

属性を使用して、データベースへのクラスとプロパティのマッピング方法を制御することもできます。 フィールドにミドル ネームも含まれている場合があるため、名フィールドに対して `FirstMidName` という名前を使用したとします。 ただし、データベース列は `FirstName` という名前にする必要があります。これは、データベースに対するアドホック クエリを記述するユーザーがその名前に慣れているためです。 このマッピングを作成する場合、`Column` 属性を使用できます。

`Column` 属性は、データベースの作成時に、`FirstMidName` プロパティにマップする `Student` テーブルの列が `FirstName` という名前になるように指定します。 つまり、コードが `Student.FirstMidName` を参照したときに、データが `Student` テーブルの `FirstName` 列から取り込まれるか、更新されます。 列名を指定しない場合は、プロパティ名と同じ名前が付けられます。

*Student.cs*ファイルで、 `using` [system.componentmodel](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx)のステートメントを追加し、次の強調表示されたコードに示すように、列`FirstMidName`名の属性をプロパティに追加します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)を追加すると、schoolcontext.cs をサポートするモデルが変更されるため、データベースと一致しません。 別の移行を作成するには、PMC に次のコマンドを入力します。

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

**サーバーエクスプローラー**で、 *student テーブルを*ダブルクリックして、 *student*テーブルデザイナーを開きます。

次の図は、最初の2つの移行を適用する前の元の列名を示しています。 列名がから`FirstMidName`に`FirstName`変更されるだけでなく、2つの`MAX` name 列の長さが50文字に変更されました。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

また、このチュートリアルで後ほど説明するように、 [FLUENT API](https://msdn.microsoft.com/data/jj591617)を使用してデータベースマッピングの変更を行うこともできます。

> [!NOTE]
> 次のセクションですべてのエンティティ クラスの作成を完了する前にコンパイルしようとすると、コンパイラ エラーが発生する可能性があります。

## <a name="update-student-entity"></a>Student エンティティの更新

*Models\Student.cs*で、前の手順で追加したコードを次のコードに置き換えます。 変更が強調表示されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Required 属性

[Required 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)によって、name プロパティに必須フィールドが作成されます。 は`Required attribute` 、DateTime、int、double、float などの値型には必要ありません。 値型には null 値を割り当てることができないため、これらは本質的に必須フィールドとして扱われます。 

`MinimumLength` を適用するには、`Required` 属性を `MinimumLength` と共に使用する必要があります。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

`MinimumLength` と `Required` を使用すると、空白で検証を満たすことができます。 文字列を`RegularExpression`完全に制御するには、属性を使用します。

### <a name="the-display-attribute"></a>Display 属性

`Display` 属性は、テキスト ボックスのキャプションを、各インスタンスのプロパティ名 (単語を区切るスペースがない) の代わりに、"First Name"、"Last Name"、"Full Name"、"Enrollment Date" にするよう指定します。

### <a name="the-fullname-calculated-property"></a>FullName 計算プロパティ

`FullName` は集計プロパティであり、2 つの別のプロパティを連結して作成される値を返します。 そのため、 `get`アクセサーのみが存在し、 `FullName`データベースに列が生成されません。

## <a name="create-instructor-entity"></a>Instructor エンティティを作成する

*Models\Instructor.cs*を作成し、テンプレートコードを次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

`Student` エンティティと `Instructor` エンティティのいくつかのプロパティが同じであることに注目してください。 このシリーズの後半の[継承の実装](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)に関するチュートリアルでは、冗長さをなくすため、このコードをリファクタリングします。

複数の属性を1行に配置することができるため、次のようにインストラクタークラスを記述することもできます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>コースと OfficeAssignment のナビゲーションプロパティ

`Courses` と `OfficeAssignment` プロパティはナビゲーション プロパティです。 既に説明したように、通常は、[遅延読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)と呼ばれる Entity Framework の機能を利用できるように、[仮想](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx)として定義されます。 さらに、ナビゲーションプロパティが複数のエンティティを保持できる場合、その型は[ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx)インターフェイスを実装する必要があります。 たとえば[IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx)はなく修飾[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx)ため`IEnumerable<T>`実装していません[追加](https://msdn.microsoft.com/library/63ywd54z.aspx)。

インストラクターは任意の数のコースを教えること`Courses`ができるため、はエンティティ`Course`のコレクションとして定義されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

ビジネスルールの状態は、インストラクターは1つのオフィスのみを持つ`OfficeAssignment`ことができるため、 `OfficeAssignment`は1つの`null`エンティティとして定義されます (オフィスが割り当てられていない場合)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>OfficeAssignment エンティティを作成する

次のコードを使用して*modelmodelofficeを*作成します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

プロジェクトをビルドします。これにより、変更が保存され、コンパイラがキャッチできるコピーと貼り付けのエラーが発生していないことが確認されます。

### <a name="the-key-attribute"></a>キー属性

`Instructor`エンティティとエンティティの間に一対ゼロまたは一対一のリレーションシップがあります。 `OfficeAssignment` オフィスの割り当ては、その担当者が割り当てられているインストラクターに対してのみ存在するため、主キーは`Instructor`エンティティに対する外部キーでもあります。 ただし、このエンティティの名前`InstructorID`が`ID`または*classname* `ID`名前付け規則に従っていないため、Entity Framework は、このエンティティの主キーとしてを自動的に認識することはできません。 したがって、`Key` 属性はキーとして識別するために使用されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

また、エンティティが独自`Key`の主キーを持っていても、プロパティにまたはとは`ID`異なる名前を付ける場合は、属性を使用できます。 `classnameID` 既定では、EF はキーを非データベース生成として扱います。これは、列が識別リレーションシップ用であるためです。

### <a name="the-foreignkey-attribute"></a>ForeignKey 属性

一対ゼロまたは一対一のリレーションシップまたは2つのエンティティ間の一対一のリレーションシップ (と`OfficeAssignment` `Instructor`の間など) がある場合、EF では、リレーションシップの end がプリンシパルであるか、end が依存しているかを解決できません。 1対1のリレーションシップでは、各クラスの参照ナビゲーションプロパティがもう一方のクラスになります。 [ForeignKey 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)を依存クラスに適用して、リレーションシップを確立できます。 [ForeignKey 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)を省略した場合、移行を作成しようとすると次のエラーが表示されます。

*型 ' ContosoUniversity ' と ' ContosoUniversity ' との間のアソシエーションのプリンシパル end を特定できませんでした。このアソシエーションのプリンシパル end は、リレーションシップ fluent API またはデータ注釈を使用して明示的に構成する必要があります。*

このチュートリアルの後半では、このリレーションシップを fluent API と構成する方法について説明します。

### <a name="the-instructor-navigation-property"></a>インストラクターナビゲーションプロパティ

エンティティには null 許容`OfficeAssignment`のナビゲーションプロパティがあります (インストラクターがオフィスの割り当てを持ってい`OfficeAssignment`ない可能性があるため) `Instructor` 。また、エンティティには null 非許容のナビゲーションプロパティがあります (オフィスの割り当てはできません)。 `Instructor`インストラクターなしで存在します`InstructorID` --null 非許容)。 エンティティに関連`OfficeAssignment`エンティティがある場合、各エンティティにはそのナビゲーションプロパティの別のエンティティへの参照が含まれます。 `Instructor`

インストラクターのナビゲーションプロパティ`[Required]`に属性を設定して、関連するインストラクターが必要であることを指定できますが、これを行う必要はありません。これは、InstructorID 外部キー (このテーブルのキーでもあります) が null 非許容であるためです。

## <a name="modify-the-course-entity"></a>Course エンティティを変更する

*Models\Course.cs*で、前の手順で追加したコードを次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Course エンティティには、関連`DepartmentID` `Department` `Department`エンティティを指す外部キープロパティがあり、ナビゲーションプロパティがあります。 Entity Framework では、関連エンティティのナビゲーション プロパティがある場合、ユーザーがデータ モデルに外部キー プロパティを追加する必要はありません。 EF では、必要に応じて、データベースに外部キーが自動的に作成されます。 ただし、データ モデルに外部キーがある場合は、更新をより簡単かつ効率的に行うことができます。 たとえば、編集`Department`する course エンティティをフェッチするときに、エンティティが読み込まれない場合は null になります。そのため、course エンティティを更新する場合は、最初`Department`にエンティティをフェッチする必要があります。 データモデルに外部キー `DepartmentID`プロパティが含まれている場合は、更新する前に`Department`エンティティをフェッチする必要はありません。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 属性

`CourseID`プロパティで[None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)パラメーターを指定して[databasegenerated 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)を指定すると、主キーの値がデータベースで生成されるのではなく、ユーザーによって提供されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

既定では、Entity Framework は、主キー値がデータベースによって生成されることを前提としています。 これはほとんどのシナリオに該当します。 ただし、エンティティ`Course`の場合は、1つの部門に対して1000シリーズ、別の部門に対して2000シリーズなど、ユーザーが指定したコース番号を使用します。

### <a name="foreign-key-and-navigation-properties"></a>外部キーとナビゲーションプロパティ

`Course`エンティティの外部キープロパティとナビゲーションプロパティには、次のリレーションシップが反映されます。

- コースは 1 つの学科に割り当てられます。したがって、前述の理由により、`DepartmentID` 外部キーと `Department` ナビゲーション プロパティが存在します。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- コースには任意の数の学生が登録できるため、`Enrollments` ナビゲーション プロパティはコレクションとなります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- コースは複数の講師が担当する場合があるため、`Instructors` ナビゲーション プロパティはコレクションとなります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Department エンティティを作成する

次のコードを使用して*Models\Department.cs*を作成します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Column 属性

以前は、列の[属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)を使用して列名のマッピングを変更していました。 エンティティのコードでは、この属性を使用してSQLデータ型のマッピングを変更し、データベースのSQLServer [money](https://msdn.microsoft.com/library/ms179882.aspx) 型を使用して列が定義されるようにしてい`Column`ます。`Department`

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

通常、Entity Framework は、プロパティに対して定義する CLR 型に基づいて適切な SQL Server データ型を選択するため、列マッピングは必要ありません。 CLR `decimal` 型は SQL Server の `decimal` 型にマップされます。 しかし、このケースでは、列に通貨の金額が保持され、 [money](https://msdn.microsoft.com/library/ms179882.aspx)データ型がより適切であることがわかっています。 CLR データ型と SQL Server データ型との一致の詳細については、「 [Entity frameworktypes の SqlClient](https://msdn.microsoft.com/library/bb896344.aspx)」を参照してください。

### <a name="foreign-key-and-navigation-properties"></a>外部キーとナビゲーションプロパティ

外部キーおよびナビゲーション プロパティには、次のリレーションシップが反映されます。

- 学科には管理者が存在する場合とそうでない場合があり、管理者は常に講師となります。 したがって、 `Instructor` `int`プロパティはエンティティへの外部キーとして含まれ、型指定の後に疑問符が追加され、プロパティが nullable としてマークされます。 `InstructorID`ナビゲーションプロパティはという`Administrator`名前ですが`Instructor` 、エンティティを保持します。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- 学科には多数のコースがあるため`Courses` 、ナビゲーションプロパティがあります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > 規則により、Entity Framework では null 非許容の外部キーと多対多リレーションシップに対して連鎖削除が有効になります。 これにより、循環連鎖削除規則が適用される可能性があり、移行を追加しようとすると例外が発生します。 たとえば、 `Department.InstructorID`プロパティを nullable として定義しなかった場合、次の例外メッセージが表示されます。"参照関係により、循環参照が許可されません。" ビジネスルールで null 非`InstructorID`許容のプロパティが必要な場合は、次の fluent API ステートメントを使用して、リレーションシップの cascade delete を無効にする必要があります。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Enrollment エンティティを変更する

 *Models\Enrollment.cs*で、前の手順で追加したコードを次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>外部キーとナビゲーションプロパティ

外部キー プロパティとナビゲーション プロパティには、次のリレーションシップが反映されます。

- 登録レコードは単一のコースに対するものであるため、`CourseID` 外部キー プロパティと `Course` ナビゲーション プロパティがあります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- 登録レコードは 1 人の学生に対するものであるため、`StudentID` 外部キー プロパティと `Student` ナビゲーション プロパティがあります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>多対多リレーションシップ

`Student`エンティティと`Course`エンティティの間には多対多のリレーションシップがあり、エンティティ`Enrollment`はデータベースの*ペイロードを含む*多対多の結合テーブルとして機能します。 つまり、 `Enrollment`テーブルには、結合されたテーブルの外部キー (この場合は主キー `Grade`とプロパティ) に加えて、追加のデータが含まれます。

次の図は、エンティティ図でこれらのリレーションシップがどのようになるかを示しています (この図は[Entity Framework パワーツール](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)を使用して生成されたもので、図の作成はチュートリアルの一部ではなく、ここでは図として使用されています)。

![学生-Course_many-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

各関係線には、1つの端に1が\*あり、もう一方にアスタリスク () があり、一対多のリレーションシップを示しています。

テーブルに`Enrollment`グレード情報が含まれていない場合は、2つの外部キー `CourseID`と`StudentID`が含まれている必要があります。 この場合、データベース内の*ペイロード*(または*純粋結合テーブル*) のない多対多の結合テーブルに対応しているので、モデルクラスをまったく作成する必要はありません。 エンティティ`Instructor` と`Course`エンティティには、このような多対多リレーションシップがあります。ご覧のように、エンティティクラスは存在しません。

![Course_many-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

ただし、次のデータベースダイアグラムに示すように、データベースには結合テーブルが必要です。

![Course_many-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework によってテーブル`CourseInstructor`が自動的に作成され、および`Course.Instructors`ナビゲーションプロパティの`Instructor.Courses`読み取りと更新によって間接的に読み取りと更新が行われます。

## <a name="entity-relationship-diagram"></a>エンティティリレーションシップダイアグラム

次の図では、完成した School モデルに対して Entity Framework Power Tools で作成される図を示します。

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

多対\*多のリレーションシップライン (to \* \*) と一対多のリレーションシップ線 (1 対) に加えて、と`OfficeAssignment`の`Instructor`間に一対ゼロまたは一対一の関係線 (1 対 0 ..1) を表示できます。インストラクターエンティティと Department エンティティの間のエンティティと、0または一対多のリレーションシップライン ( \*0 ..1)。

## <a name="add-code-to-database-context"></a>データベースコンテキストへのコードの追加

次に、新しいエンティティを`SchoolContext`クラスに追加し、 [fluent API](https://msdn.microsoft.com/data/jj591617)呼び出しを使用してマッピングの一部をカスタマイズします。 API は、次の例のように、一連のメソッド呼び出しを1つのステートメントに組み合わせるすることによって頻繁に使用されるため、"fluent" になります。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

このチュートリアルでは、属性では実行できないデータベースマッピングに対してのみ fluent API を使用します。 ただし、fluent API を使用して、属性で実行できる書式設定、検証、およびマッピング規則のほとんどを指定することもできます。 `MinimumLength` などの一部の属性は fluent API で適用できません。 前述のように`MinimumLength` 、はスキーマを変更しません。クライアント側とサーバー側の検証規則のみを適用します。

一部の開発者は fluent API のみを使用することを選ぶため、エンティティ クラスを "クリーン" な状態に保つことができます。 必要に応じて、属性と fluent API を組み合わせて使用することができます。fluent API のみを使用して実行できるカスタマイズがいくつかありますが、一般的は 2 つの方法のいずれかを選択して、できるだけ一貫性を保つためにそれを使用することをお勧めします。

新しいエンティティをデータモデルに追加し、属性を使用して実行していないデータベースマッピングを実行するには、 *DAL\SchoolContext.cs*のコードを次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

[Onmodelcreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)メソッドの新しいステートメントによって、多対多の結合テーブルが構成されます。

- エンティティ`Instructor` と`Course`エンティティの間の多対多リレーションシップの場合、コードでは結合テーブルのテーブル名と列名を指定します。 このコードを使用せずに多対多リレーションシップを構成 Code First ことができますが、このコードを呼び出さないと、 `InstructorInstructorID` `InstructorID`列のなどの既定の名前が取得されます。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

次のコードは、属性ではなく fluent API を使用`Instructor`して、エンティティと`OfficeAssignment`エンティティの間のリレーションシップを指定する方法の例を示しています。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

バックグラウンドで実行されている "fluent API" ステートメントの詳細については、「 [FLUENT API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) 」ブログの投稿を参照してください。

## <a name="seed-database-with-test-data"></a>テスト データを使ってデータベースをシードする

作成した新しいエンティティのシードデータを提供するために、 *migrations\ configuration.cs*ファイルのコードを次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

最初のチュートリアルで見たように、このコードのほとんどは、新しいエンティティオブジェクトを更新または作成し、テストに必要なときにサンプルデータをプロパティに読み込みます。 ただし、エンティティ`Instructor`との`Course`多対多リレーションシップを持つエンティティは、次のように処理されることに注意してください。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

オブジェクトを`Course`作成するときに、コード`Instructors = new List<Instructor>()`を`Instructors`使用して、ナビゲーションプロパティを空のコレクションとして初期化します。 これにより、メソッドを`Instructor`使用して、 `Instructors.Add`この`Course`に関連するエンティティを追加できるようになります。 空のリストを作成していない場合、プロパティは`Instructors` null であり、 `Add`メソッドがないため、これらのリレーションシップを追加することはできません。 また、リストの初期化をコンストラクターに追加することもできます。

## <a name="add-a-migration"></a>移行を追加する

PMC から、 `add-migration`コマンドを入力します ( `update-database`コマンドはまだ実行しないでください)。

`add-Migration ComplexDataModel`

この時点で `update-database` コマンドを実行しようとした場合は (まだ実行しないでください)、以下のようなエラーが表示されます。

*ALTER TABLE ステートメントが外部キー制約 "FK\_dbo" と競合しています。コース\_dbo。Department\_DepartmentID "。データベース "ContosoUniversity"、テーブル "dbo で競合が発生しました。Department ", column ' DepartmentID '.*

既存のデータを使用して移行を実行する場合、外部キー制約を満たすスタブデータをデータベースに挿入する必要があることがあります。これを行う必要があります。 ComplexDataModel `Up`メソッドで生成されたコードは、null 非`DepartmentID`許容の外部キー `Course`をテーブルに追加します。 コードの実行`Course` `AddColumn`時にはテーブルに既に行があるため、null にできない列に配置する値が SQL Server で認識されないため、操作は失敗します。 そのため、新しい列に既定値を指定するようにコードを変更し、既定の学科として機能する "Temp" という名前のスタブ学科を作成する必要があります。 その結果、メソッドの`Course` `Up`実行後に、既存の行はすべて "Temp" 学科に関連付けられます。 `Seed`メソッド内の適切な部門に関連付けることができます。

&lt; *TimestampComplexDataModel.cs\_ファイルを編集し、DepartmentID 列が Course テーブルに追加されたコード行をコメントアウトして、次の強調表示されたコードを追加します (コメント付きの行も同様です)。&gt;* 強調表示):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

メソッドを実行すると、 `Department`テーブルに行が挿入され、既存`Course`の行がその新しい`Department`行に関連付けられます。 `Seed` UI でコースを追加していない場合は、 `Course.DepartmentID`列の "Temp" 部署または既定値が不要になります。 アプリケーションを使用して他のユーザーがコースを追加した可能性を実現するには、 `Seed`メソッドのコードを更新して、( `Seed`メソッドの以前の実行で挿入されたものだけでなく) すべて`Course`の行があることを確認する必要もあります。列`DepartmentID`から既定値を削除して "Temp" 部署を削除する前の有効な値。

## <a name="update-the-database"></a>データベースを更新する

&lt; `update-database` *タイムスタンプ\_の ComplexDataModel.cs ファイルの編集が完了したら、PMC にコマンドを入力して移行を実行します。&gt;*

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> データを移行してスキーマを変更するときに、他のエラーが発生する可能性があります。 解決できない移行エラーが発生した場合は、接続文字列のデータベース名を変更するか、データベースを削除できます。 最も簡単な方法は、 *web.config ファイル内のデータベース*の名前を変更することです。 次の例では、名前が CU\_テストに変更されたことを示しています。
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> 新しいデータベースでは、移行`update-database`するデータがないため、コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除する方法については、「 [Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)」を参照してください。
>
> それでも失敗する場合は、PMC で次のコマンドを入力して、データベースを再初期化することをお勧めします。
>
> `update-database -TargetMigration:0`

先ほどと同じように**サーバーエクスプローラー**でデータベースを開き、 **[テーブル]** ノードを展開して、すべてのテーブルが作成されていることを確認します。 (それでも**サーバーエクスプローラー**がまだ開いていない場合は、 **[更新]** ボタンをクリックしてください)。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

`CourseInstructor`テーブルのモデルクラスを作成していません。 既に説明したように、これは、エンティティと`Instructor` `Course`エンティティの間の多対多リレーションシップの結合テーブルです。

`CourseInstructor`テーブルを右クリックし、 **[テーブルデータの表示]** をクリックして、 `Instructor` `Course.Instructors`ナビゲーションプロパティに追加したエンティティの結果として、テーブルにデータが含まれていることを確認します。

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>コードを取得する

[完成したプロジェクトのダウンロード](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>その他の技術情報

その他の Entity Framework リソースへのリンクについては、 [ASP.NET Data Access の推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)を参照してください。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * データモデルのカスタマイズ
> * 学生エンティティの更新
> * Instructor エンティティを作成した
> * OfficeAssignment エンティティを作成した
> * Course エンティティを変更しました
> * Department エンティティを作成しました
> * 登録エンティティが変更されました
> * データベースコンテキストへのコードの追加
> * テスト データを使ってデータベースをシードした
> * 移行を追加した
> * データベースを更新した

次の記事に進み、Entity Framework がナビゲーションプロパティに読み込む関連データを読み取り、表示する方法について説明します。

> [!div class="nextstepaction"]
> [関連データの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
