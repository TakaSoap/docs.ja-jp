---
title: 移行と相互運用性
ms.date: 03/23/2020
f1_keywords:
- AutoGeneratedOrientationPage
helpviewer_keywords:
- Windows Presentation Foundation [WPF], interoperability
- WPF [WPF], migration
- Windows Forms controls [WPF interoperability]
- controls [WPF interoperability]
- Windows Presentation Foundation [WPF], migration
- interoperability [WPF]
- WPF [WPF], interoperability
- migration [WPF]
ms.assetid: d655de05-bf63-4814-bc64-6b3be01c70a2
ms.openlocfilehash: 180ece4f178406c6c3e704ecadaa9a72955ec038
ms.sourcegitcommit: 99b153b93bf94d0fecf7c7bcecb58ac424dfa47c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/25/2020
ms.locfileid: "80249052"
---
# <a name="migration-and-interoperability"></a>移行と相互運用性

このページには、アプリケーションと他の種類の Microsoft Windows[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]アプリケーション間の相互運用を実装する方法を説明するドキュメントへのリンクが含まれています。

## <a name="in-this-section"></a>このセクションの内容

[WPF から System.Xaml に移行された型](types-migrated-from-wpf-to-system.md)\
[WPF と Windows フォームの相互運用](wpf-and-windows-forms-interoperation.md)\
[WPF と Win32 の相互運用](wpf-and-win32-interoperation.md)\
[WPF と Direct3D9 の相互運用性](wpf-and-direct3d9-interoperation.md)

## <a name="reference"></a>関連項目

| 期間                                                     | 定義                                                                                                                                                                                                                                                                                                                                                                                                  |
|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <xref:System.Windows.Forms.Integration.WindowsFormsHost> | [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] Windows フォーム コントロールをページの要素としてホストするために使用できる要素。                                                                                                                                                                                                                                      |
| <xref:System.Windows.Forms.Integration.ElementHost>      | [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)]コントロールのホストに使用できる Windows フォーム コントロール。                                                                                                                                                                                                                                                                 |
| <xref:System.Windows.Interop.HwndSource>                 | Win32 アプリケーション内の[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]領域をホストします。                                                                                                                                                                                                                                                                                |
| <xref:System.Windows.Interop.HwndHost>                   | の<xref:System.Windows.Forms.Integration.WindowsFormsHost>基本クラスは、アプリケーションによってホストされる場合に、すべての HWND ベースのテクノロジが[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]使用する基本的な機能を定義します。 これをサブクラス化して、アプリケーション内で Win32 ウィンドウを[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]ホストします。 |
| <xref:System.Windows.Interop.BrowserInteropHelper>       | ブラウザーでホストされているアプリケーションのブラウザー環境の状態を[!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]報告するためのヘルパー クラス。                                                                                                                                                                                                         |

## <a name="related-sections"></a>関連項目
