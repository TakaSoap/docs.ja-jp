---
title: '方法 : 特定の属性を持つ要素を検索する (XPath-LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: 4bb38d2c-bc7c-4196-8909-aaf41fb86b28
ms.openlocfilehash: 36f725a7684dc009dbfb956a1584fd6ca5e487b0
ms.sourcegitcommit: 43d10ef65f0f1fd6c3b515e363bde11a3fcd8d6d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/04/2020
ms.locfileid: "78267042"
---
# <a name="how-to-find-elements-with-a-specific-attribute-xpath-linq-to-xml-visual-basic"></a>方法 : 特定の属性を持つ要素を検索する (XPath LINQ から XML へ) (Visual Basic)
特定の属性を持つすべての要素を検索しなければならない場合があります。 属性の内容は問わず、 属性の存在に基づいて選択を行うような場合です。  
  
 XPath 式を次に示します。  
  
 `./*[@Select]`  
  
## <a name="example"></a>例  
 次のコードは、`Select` 属性を持つ要素のみを選択します。  
  
```vb  
Dim doc As XElement = _
    <Root>  
        <Child1>1</Child1>  
        <Child2 Select='true'>2</Child2>  
        <Child3>3</Child3>  
        <Child4 Select='true'>4</Child4>  
        <Child5>5</Child5>  
    </Root>  
  
' LINQ to XML query  
Dim list1 As IEnumerable(Of XElement) = _  
    From el In doc.Elements() _  
    Where el.@Select <> Nothing _  
    Select el  
  
' XPath expression  
Dim list2 As IEnumerable(Of XElement) = DirectCast(doc.XPathEvaluate _  
    ("./*[@Select]"), IEnumerable).Cast(Of XElement)()  
  
If list1.Count() = list2.Count() And _  
    list1.Intersect(list2).Count() = list1.Count() Then  
    Console.WriteLine("Results are identical")  
Else  
    Console.WriteLine("Results differ")  
End If  
  
For Each el As XElement In list1  
    Console.WriteLine(el)  
Next  
```  
  
 この例の結果は、次のようになります。  
  
```console
Results are identical  
<Child2 Select="true">2</Child2>  
<Child4 Select="true">4</Child4>  
```  
  
## <a name="see-also"></a>関連項目

- [XPath ユーザー用の LINQ to XML (ビジュアル ベーシック)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml-for-xpath-users.md)
