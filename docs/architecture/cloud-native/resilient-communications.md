---
title: 回復性のある通信
description: Azure 向けのクラウドネイティブ .NET アプリの設計 |回復力のある通信
ms.date: 06/30/2019
ms.openlocfilehash: 324f5426af1c892db73aa6fc2336a19b7a8e499e
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "73841223"
---
# <a name="resilient-communications"></a>回復力のある通信

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

この本では、従来のモノリシックアプリケーション設計を超えて移行し、一連の分散された自己完結型サービスが独立して実行され、それぞれと通信するマイクロサービスベースのアーキテクチャを採用することのメリットを evangelized しました。HTTP や HTTPS などの標準の通信プロトコルを使用する場合。 このようなアーキテクチャには、多くの重要な利点がありますが、多くの課題があります。 たとえば、次のような懸念事項が考えられます。

- *プロセス外のネットワーク通信。* 各サービスは、ネットワークの輻輳、待機時間、および一時的な障害が発生するネットワークプロトコルを介して通信します。
- *サービスの検出。* 各サービスがそれぞれ独自の IP アドレスとポートを持つマシンのクラスターで実行されている場合、サービスはどのようにして互いを検出して通信しますか。
- *柔軟性.* 短時間の障害を管理し、システムの安定性を維持するにはどうすればよいですか。
- *負荷分散。* 受信トラフィックは、サービスの複数のインスタンスにどのように分散されるのですか。
- *セキュリティ。* トランスポートレベルの暗号化と証明書の管理などのセキュリティの問題はどのように適用されますか。
- *分散型監視。* -複数の使用サービスにわたる1つの要求について、追跡可能性と監視を相互に関連付けてキャプチャする方法を教えてください。

これらの問題はさまざまなライブラリやフレームワークで対処できますが、コードベース内に実装すると、コストが高く複雑で時間がかかることがあります。 さらに、インフラストラクチャに関する懸念事項がビジネスロジックと関連付けられているソリューションもあります。

## <a name="service-mesh"></a>サービスメッシュ

より優れたアプローチは、*サービスメッシュ*の対象となる、急速に進化する新しいテクノロジを検討することです。 [サービスメッシュ](https://www.nginx.com/blog/what-is-a-service-mesh/)は、構成可能なインフラストラクチャレイヤーであり、サービス通信や前述の多くの課題を処理するための組み込みの機能を備えています。 これらの懸念事項をビジネスコードから切り離し、サービスプロキシ (各サービスに付随するのインスタンス) に移動します。 サービスメッシュプロキシは、サイドカー[パターン](https://docs.microsoft.com/azure/architecture/patterns/sidecar)とも呼ばれ、ビジネスコードから分離とカプセル化を行うための個別のプロセスにデプロイされます。 ただし、プロキシは、それと共に作成されるサービスと、そのライフサイクルを共有するサービスに密接にリンクされています。 図6-9 はこのシナリオを示しています。

![サイドカーを使用したサービスメッシュ](./media/service-mesh-with-side-car.png)

**図 6-9**。 サイドカーを使用したサービスメッシュ

前の図では、プロキシがマイクロサービスとクラスター間の通信を傍受して管理する方法に注意してください。

サービスメッシュは、[データプレーン](https://blog.envoyproxy.io/service-mesh-data-plane-vs-control-plane-2774e720f7fc)と[コントロールプレーン](https://blog.envoyproxy.io/service-mesh-data-plane-vs-control-plane-2774e720f7fc)の2つの異なるコンポーネントに論理的に分割されます。 図6-10 は、これらのコンポーネントとその役割を示しています。

![サービスメッシュ制御とデータプレーン](./media/istio-control-and-data-plane.png)

**図 6-10.** サービスメッシュ制御とデータプレーン

構成が完了すると、サービスメッシュが非常に機能します。 サービス探索エンドポイントから、対応するインスタンスのプールを取得できます。 その後、特定のインスタンスに要求を送信し、結果の待機時間と応答の種類を記録することができます。 メッシュでは、最近の要求の待機時間など、多くの要因に基づいて高速な応答を返す可能性の高いインスタンスを選択できます。

インスタンスが応答しない場合、または失敗した場合、メッシュは別のインスタンスで要求を再試行できます。 プールが一貫してエラーを返す場合、メッシュは負荷分散プールからそれを削除して、回復後に定期的に再試行することができます。 要求がタイムアウトになると、メッシュが失敗し、要求を再試行することができます。 メッシュは、メトリックと分散トレースの形式で動作をキャプチャします。これは、一元化されたメトリックシステムに出力できます。

## <a name="istio-and-envoy"></a>Iとエンボイ

現在、いくつかのサービスメッシュオプションが存在しますが、このドキュメントの執筆時点では、 [iと](https://istio.io/docs/concepts/what-is-istio/)して最も人気があります。 新しいまたは既存の分散アプリケーションに統合できるオープンソースのサービスであり、IBM、Google、およびその他のサービスとの共同事業です。 マイクロサービスをセキュリティで保護、接続、監視するための一貫した完全なソリューションを提供します。 その特徴は次のとおりです。

- 強力な id ベースの認証と承認を使用して、クラスター内のサービス間通信をセキュリティで保護します。
- HTTP、 [Grpc](https://grpc.io/)、WEBSOCKET、TCP トラフィックの自動負荷分散。
- 豊富なルーティング規則、再試行、フェールオーバー、およびフォールトインジェクションにより、トラフィックの動作をきめ細かく制御できます。
- アクセス制御、レート制限、クォータをサポートするプラグ可能なポリシーレイヤーと構成 API。
- クラスター内のすべてのトラフィック (クラスターの受信と送信を含む) の自動メトリック、ログ、およびトレース。

Iの実装の主要なコンポーネントは、[エンボイプロキシ](https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy)を持つプロキシサービスです。 この後、1つの章で説明されているように、後から[クラウドネイティブコンピューティングファンデーション](https://www.cncf.io/)に貢献すると、エンボイプロキシは各サービスと共に実行され、次の機能のためのプラットフォームに依存しない基礎となります。

- 動的サービスの検出。
- 負荷分散。
- TLS の終了。
- HTTP および gRPC プロキシ。
- サーキットブレーカーの回復性。
- 正常性チェック。
- [カナリア](https://martinfowler.com/bliki/CanaryRelease.html)デプロイによる更新のローリング。

既に説明したように、エンボイはクラスター内の各マイクロサービスにサイドカーとしてデプロイされます。

## <a name="integration-with-azure-kubernetes-services"></a>Azure Kubernetes Services との統合

Azure クラウドは、azure Kubernetes Services 内での直接サポートを提供します。 次のリンクを使用して作業を開始できます。

- [AKS での Ip のインストール](https://docs.microsoft.com/azure/aks/istio-install)
- [AKS と Iを使用する](https://docs.microsoft.com/azure/aks/istio-scenario-routing)

>[!div class="step-by-step"]
>[前へ](infrastructure-resiliency-azure.md)
>[次へ](monitoring-health.md)
