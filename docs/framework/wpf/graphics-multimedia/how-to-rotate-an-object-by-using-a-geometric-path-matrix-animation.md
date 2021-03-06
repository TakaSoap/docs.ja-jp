---
title: '方法 : ジオメトリック パスを使用してオブジェクトを回転させる (行列アニメーション)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- geometric paths [WPF], rotating objects by
- rotating objects by geometric paths [WPF]
- matrix animation [WPF]
ms.assetid: 877dc9aa-6bdc-4beb-8772-3efaec32c0f0
ms.openlocfilehash: 63dc873a2b840ea3f870a8c60c5691ab271c1c9f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79187409"
---
# <a name="how-to-rotate-an-object-by-using-a-geometric-path-matrix-animation"></a>方法 : ジオメトリック パスを使用してオブジェクトを回転させる (行列アニメーション)
この例では、オブジェクト<xref:System.Windows.Media.Animation.MatrixAnimationUsingPath>と a<xref:System.Windows.Media.MatrixTransform>を使用して、オブジェクトによって定義されたジオメトリパスに沿ってオブジェクト<xref:System.Windows.Media.PathGeometry>を回転 (ピボット) する方法を示します。  
  
## <a name="example"></a>例  
 オブジェクトを使用して、<xref:System.Windows.Media.Animation.MatrixAnimationUsingPath>のプロパティを<xref:System.Windows.Media.MatrixTransform.Matrix%2A>アニメーション化する例を<xref:System.Windows.Media.MatrixTransform>次に示します。 <xref:System.Windows.Media.MatrixTransform>ボタンに適用され、曲線パスに沿って移動します。 プロパティが<xref:System.Windows.Media.Animation.MatrixAnimationUsingPath.DoesRotateWithTangent%2A>に`true`設定されているため、四角形はパスの接線に沿って回転します。  
  
 [!code-xaml[PathAnimationGallery_snippet#MatrixAnimationUsingPathDoesRotateWithTangentWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_snippet/CS/matrixanimationusingpathdoesrotatewithtangentexample.xaml#matrixanimationusingpathdoesrotatewithtangentwholepage)]  
  
 [!code-csharp[PathAnimationGallery_procedural_snip#MatrixAnimationUsingPathDoesRotateWithTangentWholePage](~/samples/snippets/csharp/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/CSharp/MatrixAnimationUsingPathDoesRotateWithTangentExample.cs#matrixanimationusingpathdoesrotatewithtangentwholepage)]
 [!code-vb[PathAnimationGallery_procedural_snip#MatrixAnimationUsingPathDoesRotateWithTangentWholePage](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PathAnimationGallery_procedural_snip/VisualBasic/MatrixAnimationUsingPathDoesRotateWithTangentExample.vb#matrixanimationusingpathdoesrotatewithtangentwholepage)]  
  
 完全なサンプルについては、「[パス アニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations)」を参照してください。  
  
 前のサンプルのコード バージョンでは、<xref:System.Windows.Media.Animation.Storyboard><xref:System.Windows.Media.EllipseGeometry>アニメーションを 1 つだけ適用した場合でも、 を使用して アニメーションを作成しました。 コード内のプロパティに単一のアニメーションを適用する簡単な方法は、<xref:System.Windows.Media.Animation.Animatable.BeginAnimation%2A>メソッドを使用することです。 例については、「[ストーリーボードを使用せずにプロパティをアニメーション化する](how-to-animate-a-property-without-using-a-storyboard.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [アニメーションの概要](animation-overview.md)
- [パス アニメーションに関する「方法」トピック](path-animation-how-to-topics.md)
- [パス アニメーションのサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Animation/PathAnimations)
