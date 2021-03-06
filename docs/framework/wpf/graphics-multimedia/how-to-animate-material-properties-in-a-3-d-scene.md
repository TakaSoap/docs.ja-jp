---
title: 3D シーンでマテリアル プロパティをアニメーション化する方法
ms.date: 03/30/2017
helpviewer_keywords:
- Material properties [WPF], animating in 3D scenes
- animation [WPF], Material properties in 3D scenes
- 3D scenes [WPF], animating Material properties
ms.assetid: 229fd6eb-7401-4992-b0c9-8b28de230c0f
ms.openlocfilehash: db59debcd7558cde52d8604cb04c8c4e68921825
ms.sourcegitcommit: 267d092663aba36b6b2ea853034470aea493bfae
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80112194"
---
# <a name="how-to-animate-material-properties-in-a-3d-scene"></a>3D シーンでマテリアル プロパティをアニメーション化する方法
この例では、3D モデルに<xref:System.Windows.Media.Brush.Opacity%2A>適用されたプロパティ<xref:System.Windows.Media.Media3D.Material>をアニメートする方法を示します。  
  
 次のコード例では、3D <xref:System.Windows.Media.LinearGradientBrush> <xref:System.Windows.Media.Media3D.Material>オブジェクトに適用されるオブジェクトとして使用されるを定義します。  
  
 [!code-xaml[Animation3DGallery_snip#AnimateMaterialExampleInline1](~/samples/snippets/csharp/VS_Snippets_Wpf/Animation3DGallery_snip/CS/AnimateMaterialExample.xaml#animatematerialexampleinline1)]  
  
 この<xref:System.Windows.Media.Brush.Opacity%2A><xref:System.Windows.Media.LinearGradientBrush>プロパティは、次のコード例を使用してアニメーション化されます。  
  
 [!code-xaml[Animation3DGallery_snip#AnimateMaterialExampleInline2](~/samples/snippets/csharp/VS_Snippets_Wpf/Animation3DGallery_snip/CS/AnimateMaterialExample.xaml#animatematerialexampleinline2)]  
  
## <a name="example"></a>例  
 次のコードは、サンプル全体を示しています。  
  
 [!code-xaml[Animation3DGallery_snip#AnimateMaterialExampleWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/Animation3DGallery_snip/CS/AnimateMaterialExample.xaml#animatematerialexamplewholepage)]  
  
## <a name="see-also"></a>関連項目

- [3D シーンを作成する](how-to-create-a-3-d-scene.md)
- [3D グラフィックスの概要](3-d-graphics-overview.md)
