---
title: 2 つのシーケンスの和集合の取得
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8b8bd3cb-86d4-4a3b-9906-61f68726dd1f
ms.openlocfilehash: 058856243b2a8daaecd653a9b5999013de5407a8
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "71182528"
---
# <a name="return-the-set-union-of-two-sequences"></a>2 つのシーケンスの和集合の取得
2 つのシーケンスの和集合を返すには、<xref:System.Linq.Queryable.Union%2A> 演算子を使用します。  
  
## <a name="example"></a>例  
 この例で<xref:System.Linq.Queryable.Union%2A>は、を使用して、または`Employees`のいずれか`Customers`の国/地域のシーケンスを返します。  
  
 [!code-csharp[DLinqQueryExamples#43](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#43)]
 [!code-vb[DLinqQueryExamples#43](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#43)]  
  
 で[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]は、 [`UNION ALL`](/sql/t-sql/language-elements/set-operators-union-transact-sql)演算子は、マルチセットに対して、マルチセットの順序なしの連結 (実質的には SQL の句の結果) として定義されます。 <xref:System.Linq.Queryable.Union%2A>

詳細と例については<xref:System.Linq.Queryable.Union%2A?displayProperty=nameWithType>、「」を参照してください。
  
## <a name="see-also"></a>関連項目

- [クエリの例](query-examples.md)
- [標準クエリ演算子の変換](standard-query-operator-translation.md)
