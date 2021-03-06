---
title: '方法: ストーリーボードを使用せずにプロパティをアニメーション化する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- non-Storyboard animation
- local animation [WPF]
- animation [WPF], non-Storyboard (local)
ms.assetid: d411db70-4df7-487d-82bc-95a7c1b2e7f8
ms.openlocfilehash: 71711c0392576930e97986078ec5926ff6ca9813
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2019
ms.locfileid: "69963020"
---
# <a name="how-to-animate-a-property-without-using-a-storyboard"></a>方法: ストーリーボードを使用せずにプロパティをアニメーション化する
この例では、を使用せずに、プロパティにアニメーションを<xref:System.Windows.Media.Animation.Storyboard>適用する方法の一例を示します。  
  
> [!NOTE]
> この機能は、[!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] では使用できません。 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] でプロパティをアニメーション化する方法については、「[ストーリーボードを使ってプロパティをアニメーション化する](how-to-animate-a-property-by-using-a-storyboard.md)」を参照してください。  
  
 ローカルアニメーションをプロパティに適用するには、 <xref:System.Windows.UIElement.BeginAnimation%2A>メソッドを使用します。 このメソッドは、アニメーション化する<xref:System.Windows.DependencyProperty>プロパティを指定すると、そのプロパティに適用するアニメーションの2つのパラメーターを受け取ります。  
  
## <a name="example"></a>例  
 次の例は、の<xref:System.Windows.Controls.Button>幅と背景色をアニメーション化する方法を示しています。  
  
 [!code-cpp[animateproperty#11](~/samples/snippets/cpp/VS_Snippets_Wpf/animateproperty/CPP/LocalAnimationExample.cpp#11)]
 [!code-csharp[animateproperty#11](~/samples/snippets/csharp/VS_Snippets_Wpf/animateproperty/CSharp/LocalAnimationExample.cs#11)]
 [!code-vb[animateproperty#11](~/samples/snippets/visualbasic/VS_Snippets_Wpf/animateproperty/VisualBasic/LocalAnimationExample.vb#11)]  
  
 <xref:System.Windows.Media.Animation>名前空間のさまざまなアニメーションクラスは、さまざまな種類のプロパティをアニメーション化するために用意されています。 プロパティのアニメーション化の詳細については、「[アニメーションの概要](animation-overview.md)」を参照してください。 依存関係プロパティ (これらの例に示されているプロパティの種類) とその機能の詳細については、「[依存関係プロパティの概要](../advanced/dependency-properties-overview.md)」を参照してください。  
  
 オブジェクトを使用<xref:System.Windows.Media.Animation.Storyboard>せずにアニメーション化する方法は他にもあります。詳細については、「[プロパティアニメーションの手法の概要](property-animation-techniques-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Media.Animation.AnimationTimeline>
- <xref:System.Windows.Media.Animation.Animatable.BeginAnimation%2A>
- <xref:System.Windows.Media.Animation>
- <xref:System.Windows.Media.Animation.Storyboard>
- [プロパティ アニメーションの手法の概要](property-animation-techniques-overview.md)
- [アニメーションの概要](animation-overview.md)
