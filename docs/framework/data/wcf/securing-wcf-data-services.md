---
title: WCF Data Services のセキュリティ保護
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- securing application [WCF Data Services]
- WCF Data Services, security
ms.assetid: 99fc2baa-a040-4549-bc4d-f683d60298af
ms.openlocfilehash: e09007e786772e838a9aaa76eb8ef7a3fe2b7cca
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79174655"
---
# <a name="securing-wcf-data-services"></a>WCF Data Services のセキュリティ保護
このトピックでは、WCF データ サービスの開発、展開、および実行に固有のセキュリティ上の考慮事項と、オープン データ プロトコル (OData) をサポートするサービスにアクセスするアプリケーションについて説明します。 また、セキュリティで保護された .NET Framework アプリケーションを作成するための推奨事項に従う必要があります。  
  
WCF データ サービス ベースの OData サービスをセキュリティで保護する方法を計画する場合、認証、プリンシパルの ID を検出および検証するプロセス、および認証されたプリンシパルが認証済みプリンシパルであるかどうかを判断するプロセスの両方に対処する必要があります。要求されたリソースへのアクセスが許可されます。 また、SSL を使用してメッセージを暗号化するかどうかを検討する必要もあります。  
  
## <a name="authenticating-client-requests"></a>クライアント要求の認証  
WCF データ サービスは、独自の認証の種類を実装しませんが、むしろデータ サービス ホストの認証の規定に依存します。 つまり、サービスは、受信した要求がネットワーク ホストによって既に認証されており、ホストが WCF Data Services によって提供されるインターフェイスを介して要求の原則を適切に識別していることを前提としています。 これらの認証オプションと方法については、マルチパート[OData および認証シリーズ](https://devblogs.microsoft.com/odata/?s=%22OData+and+authentication%22)で詳しく説明しています。  
  
### <a name="authentication-options-for-a-wcf-data-service"></a>WCF Data Service の認証オプション  
 次の表に、WCF Data Service への要求を認証するために使用できる認証メカニズムを示します。  
  
|認証オプション|説明|  
|----------------------------|-----------------|  
|匿名認証|匿名 HTTP 認証が有効になっている場合は、すべてのプリンシパルがデータ サービスに接続できます。 匿名アクセスには資格情報は必要ありません。 このオプションは、だれでもデータ サービスにアクセスできるようにする場合にのみ使用します。|  
|基本認証とダイジェスト認証|ユーザー名とパスワードで構成される資格情報が認証に必要です。 Windows 以外のクライアントの認証がサポートされます。 **セキュリティに関する注意事項:** 基本認証資格情報 (ユーザー名とパスワード) はクリアで送信され、傍受される可能性があります。 ダイジェスト認証では、指定された資格情報に基づくハッシュが送信されるため、基本認証に比べて安全です。 ただし、どちらの方法も man-in-the-middle 攻撃を受ける可能性があります。 これらの認証方法を使用する場合は、SSL (Secure Sockets Layer) を使用してクライアントとデータ サービスの間の通信を暗号化することを検討してください。 <br /><br /> Microsoft インターネット インフォメーション サービス (IIS) では、ASP.NET アプリケーションで HTTP 要求の基本認証とダイジェスト認証の両方を実装します。 この Windows 認証プロバイダーの実装を使用すると、.NET Framework クライアント アプリケーションで、資格情報を要求の HTTP ヘッダーでデータ サービスに渡して Windows ユーザーの認証をシームレスにネゴシエートできます。 詳細については、「[ダイジェスト認証テクニカル リファレンス](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc782794(v=ws.10))」を参照してください。<br /><br /> Windows 資格情報ではなくカスタム認証サービスに基づいてデータ サービスに基本認証を使用させるには、認証用にカスタム ADP.NET HTTP モジュールを実装する必要があります。<br /><br /> WCF Data Services でカスタム基本認証スキームを使用する方法の例については、ブログ記事[「OData と認証 - パート 6 – カスタム基本認証](https://devblogs.microsoft.com/odata/odata-and-authentication-part-6-custom-basic-authentication/)」を参照してください。|  
|Windows 認証|Windows ベースの資格情報の交換には、NTLM または Kerberos が使用されます。 このメカニズムは基本認証やダイジェスト認証より安全ですが、クライアントが Windows ベースのアプリケーションである必要があります。 IIS では、ASP.NET アプリケーションで HTTP 要求に対する Windows 認証の実装も提供されます。 詳細については、「[フォーム認証の概要ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/7t6b43z4(v=vs.100))」を参照してください。<br /><br /> WCF データ サービスで Windows 認証を使用する方法の例については、ブログの記事[「OData と認証 - パート 2 – Windows 認証](https://devblogs.microsoft.com/odata/odata-and-authentication-part-2-windows-authentication/)」を参照してください。|  
|フォーム認証のASP.NET|フォーム認証では、独自のコードを使用してユーザーを認証し、認証トークンをクッキーまたはページ URL に保存できます。 作成したログイン フォームを使用してユーザーのユーザー名とパスワードを認証します。 認証されない要求はログイン ページにリダイレクトされ、ここでユーザーが資格情報を入力してフォームを送信します。 アプリケーションで要求を認証する場合、後続の要求で使用する ID を再確立するためのキーが含まれたチケットをシステムが発行します。 詳細については、「[フォーム認証プロバイダ](https://docs.microsoft.com/previous-versions/aspnet/9wff0kyh(v=vs.100))」を参照してください。 **セキュリティに関する注意事項:** 既定では、フォーム認証チケットを含む cookie は、ASP.NET Web アプリケーションでフォーム認証を使用する場合はセキュリティで保護されません。 認証チケットと最初のログイン資格情報の両方を保護するには、SSL を要求することを検討してください。 <br /><br /> WCF データ サービスでフォーム認証を使用する方法の例については、ブログの記事[「OData と認証 - パート 7 – フォーム認証」を](https://devblogs.microsoft.com/odata/odata-and-authentication-part-7-forms-authentication/)参照してください。|  
|クレーム ベースの認証|クレームベース認証では、データ サービスは信頼された "サードパーティの" ID プロバイダー サービスを使用してユーザーを認証します。 ID プロバイダーは、データ サービス リソースへのアクセスを要求しているユーザーを確実に認証し、要求されたリソースへのアクセスを許可するトークンを発行します。 このトークンがデータ サービスに提示され、そのアクセス トークンを発行した識別情報サービスとの信頼関係に基づいてユーザーのアクセスが許可されます。<br /><br /> クレーム ベースの認証プロバイダーの利点は、信頼ドメイン間でさまざまな種類のクライアントの認証に使用できることです。 このようなサードパーティ プロバイダーを使用することにより、ユーザーの管理と認証の要件をデータ サービスからオフロードできます。 OAuth 2.0 は、サービスとしてのフェデレーション認証のために Microsoft Azure AppFabric アクセス制御によってサポートされているクレーム ベースの認証プロトコルであり、 REST ベースのサービスをサポートします。 WCF データ サービスで OAuth 2.0 を使用する方法の例については、ブログ記事[「OData と認証 - パート 8 – OAuth WRAP](https://devblogs.microsoft.com/odata/odata-and-authentication-part-8-oauth-wrap/)」を参照してください。|  
  
<a name="clientAuthentication"></a>
### <a name="authentication-in-the-client-library"></a>クライアント ライブラリの認証  
 既定では、WCF データ サービス クライアント ライブラリは、OData サービスに要求を行うときに資格情報を提供しません。 データ サービスでユーザー認証のためにログイン資格情報が要求されている場合は、<xref:System.Net.NetworkCredential> の <xref:System.Data.Services.Client.DataServiceContext.Credentials%2A> プロパティからアクセスできる <xref:System.Data.Services.Client.DataServiceContext> で資格情報を渡すことができます。以下に例を示します。  
  
```csharp  
// Set the client authentication credentials.  
context.Credentials =  
    new NetworkCredential(userName, password, domain);  
```  
  
```vb  
' Set the client authentication credentials.  
context.Credentials = _  
    New NetworkCredential(userName, password, domain)  
```  
  
 詳細については、「[方法 : データ サービス要求のクライアント資格情報を指定する](specify-client-creds-for-a-data-service-request-wcf.md)」を参照してください。  
  
 クレーム ベースのトークンやクッキーなど、<xref:System.Net.NetworkCredential> オブジェクトでは指定できないログイン資格情報がデータ サービスで要求されている場合は、手動で HTTP 要求のヘッダー (通常は `Authorization` と `Cookie`) を設定する必要があります。 この種の認証シナリオの詳細については、ブログ記事[「OData と認証 - パート 3 – ClientSide フック](https://devblogs.microsoft.com/odata/odata-and-authentication-part-3-clientside-hooks/)」を参照してください。 要求メッセージで HTTP ヘッダーを設定する方法の例については、「[方法 : クライアント要求でヘッダーを設定する](how-to-set-headers-in-the-client-request-wcf-data-services.md)」を参照してください。  
  
## <a name="impersonation"></a>権限借用  
 データ サービスは通常、データ サービスをホストしているワーカー プロセスの資格情報を使用して、要求されたリソース (サーバー上のファイル、データベースなど) にアクセスします。 偽装を使用する場合、ASP.NETアプリケーションは、要求を行うユーザーの Windows ID (ユーザー アカウント) を使用して実行できます。 偽装は、IIS を使用してユーザーを認証するアプリケーションで使用されるのが一般的です。この場合、要求されたリソースへのアクセスにそのプリンシパルの資格情報が使用されます。 詳細については、「[偽装ASP.NET](https://docs.microsoft.com/previous-versions/aspnet/xh507fc5(v=vs.100))」を参照してください。  
  
## <a name="configuring-data-service-authorization"></a>データ サービスの承認の構成  
 承認とは、事前に行われた認証に基づいて識別されるプリンシパルまたはプロセスにアプリケーション リソースへのアクセスを許可することです。 一般に、データ サービスのユーザーには、クライアント アプリケーションで必要とされている操作を実行するのに十分な権限のみを与えるようにします。  
  
### <a name="restrict-access-to-data-service-resources"></a>データ サービス リソースへのアクセスの制限  
 既定では、WCF Data Services を使用すると、データ サービス リソース (エンティティ セットとサービス操作) に対する共通の読み取りおよび書き込みアクセスを、データ サービスにアクセスできるすべてのユーザーに付与できます。 読み取りおよび書き込みのアクセスを定義する規則は、データ サービスによって公開される各エンティティ セットおよびサービス操作に対して個別に定義できます。 読み取りおよび書き込みの両方のアクセスを、クライアント アプリケーションで必要なリソースのみに制限することをお勧めします。 詳細については、「[リソース アクセスの最小要件](configuring-the-data-service-wcf-data-services.md#accessRequirements)」を参照してください。  
  
### <a name="implement-role-based-interceptors"></a>ロール ベースのインターセプターの実装  
 インターセプターを使用すると、データ サービス リソースに対する要求を、データ サービスによって処理される前にインターセプトすることができます。 詳細については、「[インターセプタ](interceptors-wcf-data-services.md)」を参照してください。 インターセプターを使用すると、要求を行っている認証されたユーザーに基づいて承認決定を行うことができます。 認証されたユーザー ID に基づいてデータ サービス リソースへのアクセスを制限する方法の例については、「[方法 : データ サービス メッセージをインターセプトする](how-to-intercept-data-service-messages-wcf-data-services.md)」を参照してください。  
  
### <a name="restrict-access-to-the-persisted-data-store-and-local-resources"></a>永続化データ ストアとローカル リソースへのアクセスの制限  
 永続化ストアにアクセスするためのアカウントには、データベースまたはファイル システムでデータ サービスの要件をサポートするのに十分な権限のみを与えるようにしてください。 匿名認証が使用されている場合は、そのアカウントがホスト アプリケーションの実行に使用されます。 詳細については、「 [How to: Develop a WCF Data Service Running on IIS](how-to-develop-a-wcf-data-service-running-on-iis.md)」を参照してください。 偽装を使用する場合は、認証されたユーザーにそれらのリソースへのアクセスを (通常は Windows グループの一部として) 許可する必要があります。  
  
## <a name="other-security-considerations"></a>その他のセキュリティの考慮事項  
  
### <a name="secure-the-data-in-the-payload"></a>ペイロードのデータの保護  
OData は HTTP プロトコルに基づいています。 データ サービスで実装されている認証によっては、HTTP メッセージのヘッダーに重要なユーザー資格情報が含まれている場合があります。 メッセージの本文にも、保護する必要がある貴重な顧客データが含まれている場合があります。 いずれの場合も、SSL を使用してその情報をネットワークで保護することをお勧めします。  
  
### <a name="ignored-message-headers-and-cookies"></a>無視されるメッセージ ヘッダーとクッキー  
 HTTP 要求のヘッダーは、コンテンツ タイプとリソースの場所を宣言するもの以外は無視されます。また、データ サービスによって設定されることはありません。  
  
 cookie は、ASP.NET フォーム認証などの認証スキームの一部として使用できます。 ただし、着信要求に設定された HTTP クッキーは、WCF データ サービスによって無視されます。 データ サービスのホストは Cookie を処理できますが、WCF データ サービス ランタイムは Cookie を分析または返しません。 WCF データ サービス クライアント ライブラリは、応答で送信された Cookie も処理しません。  
  
### <a name="custom-hosting-requirements"></a>カスタム ホストの要件  
 既定では、WCF データ サービスは、IIS でホストされているASP.NETアプリケーションとして作成されます。 そのため、データ サービスでこのプラットフォームのセキュリティ動作を活用できます。 カスタム ホストによってホストされる WCF データ サービスを定義できます。 詳細については、「[データ サービスのホスト](hosting-the-data-service-wcf-data-services.md)」を参照してください。 データ サービスをホストするコンポーネントとプラットフォームでは、データ サービスへの攻撃を防ぐために次のセキュリティ動作を確保する必要があります。  
  
- 可能なすべての操作について、データ サービス要求で受け入れる URI の長さを制限します。  
  
- 着信および発信の両方の HTTP メッセージのサイズを制限します。  
  
- 任意の時点の未処理の要求の総数を制限します。  
  
- HTTP ヘッダーとその値のサイズを制限し、WCF データ サービスのヘッダー データへのアクセスを提供します。  
  
- TCP SYN 攻撃やメッセージ リプレイ攻撃などの既知の攻撃を検出して阻止します。  
  
### <a name="values-are-not-further-encoded"></a>値のさらなるエンコード  
 データ サービスに送信されるプロパティ値は、WCF データ サービス ランタイムによってエンコードされません。 たとえば、書式設定された HTML コンテンツがエンティティの文字列プロパティに含まれていた場合、データ サービスによってタグが HTML エンコードされることはありません。 応答のプロパティ値についても、データ サービスによってさらなるエンコードが行われることはありません。 クライアント ライブラリで追加のエンコードが行われることもありません。  
  
### <a name="considerations-for-client-applications"></a>クライアント アプリケーションに関する注意点  
 次のセキュリティ上の考慮事項は、OData サービスにアクセスする WCF データ サービス クライアントを使用するアプリケーションに適用されます。  
  
- クライアント ライブラリは、データ サービスへのアクセスに使用されるプロトコルで十分なレベルのセキュリティが確保されていることを前提としています。  
  
- クライアント ライブラリは、基になるプラットフォームによって提供されるトランスポート スタックのタイムアウトや解析のすべてのオプションに対して既定値を使用します。  
  
- クライアント ライブラリはアプリケーション構成ファイルの設定を読み取りません。  
  
- クライアント ライブラリにはドメイン間アクセスのメカニズムは実装されていません。 基になる HTTP スタックによって提供されるメカニズムが使用されます。  
  
- クライアント ライブラリにはユーザー インターフェイス要素はありません。送受信されるデータの表示は行われません。  
  
- クライアント アプリケーションで常にユーザー入力と信頼されていないサービスからのデータを検証することをお勧めします。  
  
## <a name="see-also"></a>関連項目

- [WCF Data Services の定義](defining-wcf-data-services.md)
- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
