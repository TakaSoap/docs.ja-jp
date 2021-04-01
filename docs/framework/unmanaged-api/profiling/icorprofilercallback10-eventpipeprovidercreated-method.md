---
description: '詳細情報: ICorProfilerCallback10::EventPipeProviderCreated メソッド'
title: ICorProfilerCallback10::EventPipeProviderCreated メソッド
ms.date: 03/19/2021
api_name:
- ICorProfilerCallback10.EventPipeProviderCreated
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 4602438148a369f2a2321a2ec2e959cc375e37cf
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805585"
---
# <a name="icorprofilercallback10eventpipeprovidercreated-method"></a>ICorProfilerCallback10::EventPipeProviderCreated メソッド

EventPipe プロバイダーが作成されるたびにプロファイラーに通知します。
  
## <a name="syntax"></a>構文  
  
```cpp  
    HRESULT EventPipeProviderCreated([in] EVENTPIPE_PROVIDER provider); 
```  
  
## <a name="parameters"></a>パラメーター

`provider` [入力] 作成されたプロバイダー。

## <a name="requirements"></a>必要条件  

**プラットフォーム:** [.NET Core がサポートされているオペレーティング システム](../../../core/install/windows.md?pivots=os-windows)に関するページを参照してください。  
**ヘッダー** : CorProf.idl、CorProf.h  
**.NET のバージョン:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]  
  
## <a name="see-also"></a>関連項目

- [プロファイリングのインターフェイス](profiling-interfaces.md)
- [ICorProfilerCallback10 インターフェイス](icorprofilercallback10-interface.md)
- [ICorProfilerInfo12 インターフェイス](icorprofilerinfo12-interface.md)
- [ICorProfilerInfo12.EventPipeAddProviderToSession メソッド](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)