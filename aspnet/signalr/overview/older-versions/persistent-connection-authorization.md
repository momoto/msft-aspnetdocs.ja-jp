---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: SignalR 永続的な接続の認証と承認 (SignalR 1.x) |Microsoft Docs
author: bradygaster
description: このトピックでは、永続的な接続の承認を適用する方法を説明します。 概要については、SignalR アプリケーションでは、セキュリティと統合しています.
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65117040"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>SignalR 永続的接続の認証と承認 (SignalR 1.x)

によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> このトピックでは、永続的な接続の承認を適用する方法を説明します。 SignalR アプリケーションへのセキュリティの統合の詳細については、次を参照してください。[セキュリティの概要](index.md)します。

## <a name="enforce-authorization"></a>承認を適用します。

使用する場合は、承認規則を適用する、 [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)オーバーライドする必要があります、`AuthorizeRequest`メソッド。 使用することはできません、`Authorize`永続的な接続を持つ属性です。 `AuthorizeRequest`メソッドは要求ごとに、要求された操作を実行するユーザーが許可されていることを確認する前に SignalR フレームワークによって呼び出されます。 `AuthorizeRequest`メソッドは、クライアントからは呼び出されません。 代わりに、アプリケーションの標準的な認証メカニズムを通じてユーザーを認証します。

次の例では、認証されたユーザーへの要求に制限する方法を示します。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

AuthorizeRequest メソッドで、カスタマイズされた承認ロジックを追加することができます。次のように、ユーザーが特定のロールに属しているかどうかを確認しています。
