---
title: セッションでキューに置かれたメッセージのグループ化
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- queues [WCF]. grouping messages
ms.assetid: 63b23b36-261f-4c37-99a2-cc323cd72a1a
ms.openlocfilehash: 231310e5c427f507141e3c144cb02b8e848d4fbf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79185152"
---
# <a name="grouping-queued-messages-in-a-session"></a>セッションでキューに置かれたメッセージのグループ化
Windows 通信基盤 (WCF) では、1 つの受信側アプリケーションで処理するために、関連するメッセージのセットをグループ化できるセッションを提供します。 セッションに含まれるメッセージは、同じトランザクションに含まれる必要があります。 すべてのメッセージが同じトランザクションに含まれるため、1 つのメッセージの処理が失敗すると、セッション全体がロールバックされます。 各セッションは、配信不能キューや有害キューに関してよく似た動作をします。 キューに置かれたバインディングに設定される有効期間 (TTL: Time To Live) プロパティがセッションに構成されている場合は、セッション全体に適用されます。 したがって、TTL が切れる前にセッション内の一部のメッセージが送信された場合は、セッション全体が配信不能キューに配置されます。 同様に、アプリケーション キューからアプリケーションにセッション内のメッセージを送信できなかった場合は、セッション全体が有害キューに配置されます (有害キューを使用できる場合)。  
  
## <a name="message-grouping-example"></a>メッセージのグループ化の例  
 メッセージのグループ化が役立つ例の 1 つは、注文処理アプリケーションを WCF サービスとして実装する場合です。 たとえば、クライアントがこのアプリケーションに多数の項目を含む注文を送信するとします。 このクライアントは、項目ごとにサービスを呼び出すため、個別のメッセージが送信されることになります。 このため、最初の項目はサーバー A で受信され、2 番目の項目はサーバー B で受信される可能性があります。 項目が追加されるたびに、この項目を処理するサーバーは適切な注文を見つけて項目を追加する必要があるため、効率が非常に悪くなります。 すべての要求を 1 台のサーバーのみで処理する場合でも、現在処理中のすべての注文をこのサーバーによって常に把握し、新しい項目がどの注文に属するものなのかを判別する必要があるため、同様の非効率が生じます。 単一の注文に属するすべての要求をグループ化すると、このようなアプリケーションの実装は大幅に簡素化されます。 1 つの注文に属するすべての項目が 1 セッションとしてクライアント アプリケーションから送信されるため、サービスは注文を処理するときにセッション全体を 1 回で処理できます。 \  
  
## <a name="procedures"></a>手順  
  
#### <a name="to-set-up-a-service-contract-to-use-sessions"></a>セッションを使用するようにサービス コントラクトを設定するには  
  
1. セッションを必要とするサービス コントラクトを定義します。 属性を指定<xref:System.ServiceModel.ServiceContractAttribute>して、この操作を行います。  
  
    ```csharp
    SessionMode=SessionMode.Required  
    ```  
  
2. これらのメソッドは何も返さないため、コントラクト内の操作を一方向としてマークします。 これは、属性で次<xref:System.ServiceModel.OperationContractAttribute>のように指定することで行われます。  
  
    ```csharp  
    [OperationContract(IsOneWay = true)]  
    ```  
  
3. サービス コントラクトを実装し、<xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode> に <xref:System.ServiceModel.InstanceContextMode.PerSession?displayProperty=nameWithType> を指定します。 これにより、セッションごとに 1 回だけサービスがインスタンス化されます。  
  
    ```csharp  
    [ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]  
    ```  
  
4. 各サービス操作には、トランザクションが必要になります。 これを指定するには <xref:System.ServiceModel.OperationBehaviorAttribute> 属性を使用します。 トランザクションを完了する操作では、<xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete> を `true` に設定する必要があります。  
  
    ```csharp  
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    ```  
  
5. システム指定の `NetMsmqBinding` バインディングを使用するエンドポイントを構成します。  
  
6. <xref:System.Messaging> を使用してトランザクション キューを作成します。 代わりに、MSMQ (メッセージ キュー) または MMC を使用してキューを作成することもできます。 この場合、トランザクション キューを作成します。  
  
7. <xref:System.ServiceModel.ServiceHost> を使用して、サービスのサービス ホストを作成します。  
  
8. サービス ホストを開いてサービスを使用できるようにします。  
  
9. サービス ホストを閉じます。  
  
#### <a name="to-set-up-a-client"></a>クライアントを設定するには  
  
1. トランザクションのスコープを作成してトランザクション キューに書き込みます。  
  
2. サービス モデル メタデータ[ユーティリティ ツール (Svcutil.exe) ツール](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)を使用して WCF クライアントを作成します。  
  
3. 注文を行います。  
  
4. WCF クライアントを閉じます。  
  
## <a name="example"></a>例  
  
### <a name="description"></a>説明  
 次の例では、`IProcessOrder` サービス、およびこのサービスを使用するクライアントのコードを示します。 このスライドでは、WCF がキューに入っているセッションを使用してグループ化動作を提供する方法を示します。  
  
### <a name="code-for-the-service"></a>サービスのコード  
 [!code-csharp[S_Msmq_Session#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_session/cs/service.cs#1)]
 [!code-vb[S_Msmq_Session#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_session/vb/service.vb#1)]  

### <a name="code-for-the-client"></a>クライアントのコード  
 [!code-csharp[S_Msmq_Session#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_session/cs/client.cs#3)]
 [!code-vb[S_Msmq_Session#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_session/vb/client.vb#3)]  

## <a name="see-also"></a>関連項目

- [セッションとキュー](../../../../docs/framework/wcf/samples/sessions-and-queues.md)
- [キューの概要](../../../../docs/framework/wcf/feature-details/queues-overview.md)
