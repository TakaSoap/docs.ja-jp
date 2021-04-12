---
description: C# のキーワード
title: C# のキーワード
ms.date: 03/17/2021
f1_keywords:
- cs.keywords
helpviewer_keywords:
- keywords [C#]
- C# language, keywords
- Visual C#, keywords
- '@ keyword'
ms.custom: updateeachrelease
ms.openlocfilehash: 6a48d2e44a9d74bd1a756dd9e6059f57d4aa3d6d
ms.sourcegitcommit: 5ce37699c2a51ed173171813be68ef7577b1aba5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/23/2021
ms.locfileid: "104881029"
---
# <a name="c-keywords"></a>C# のキーワード

キーワードは、コンパイラで特別な意味を持つ、事前に定義されている予約済みの識別子です。 キーワードは、プレフィックスとして `@` を含めない限り、プログラムで識別子として使用できません。 たとえば、`@if` は有効な識別子ですが、`if` は違います。これは、`if` がキーワードだからです。  
  
 このトピックの最初の表では、C# プログラムの予約済み識別子であるキーワードの一覧を示します。 2 番目の表では、C# のコンテキスト キーワードの一覧を示します。 コンテキスト キーワードは、プログラムの限定されたコンテキストでのみ特別な意味を持つもので、そのコンテキストの外部では識別子として使用することができます。 一般に、C# 言語に新しいキーワードが追加されると、それらはコンテキスト キーワードとして追加されます。これは、以前のバージョンで記述されたプログラムの破損を防ぐためです。  
  
|||||  
|---|---|---|---|  
|[abstract](abstract.md)|[as](../operators/type-testing-and-cast.md#as-operator)|[base](base.md)|[bool](../builtin-types/bool.md)|  
|[break](break.md)|[byte](../builtin-types/integral-numeric-types.md)|[case](switch.md)|[catch](try-catch.md)|  
|[char](../builtin-types/char.md)|[checked](checked.md)|[class](class.md)|[const](const.md)|  
|[continue](continue.md)|[decimal](../builtin-types/floating-point-numeric-types.md)|[default](default.md)|[delegate](../builtin-types/reference-types.md)|  
|[do](do.md)|[double](../builtin-types/floating-point-numeric-types.md)|[else](if-else.md)|[enum](../builtin-types/enum.md)|  
|[event](event.md)|[explicit](../operators/user-defined-conversion-operators.md)|[extern](extern.md)|[false](../builtin-types/bool.md)|  
|[finally](try-finally.md)|[fixed](fixed-statement.md)|[float](../builtin-types/floating-point-numeric-types.md)|[for](for.md)|  
|[foreach](foreach-in.md)|[goto](goto.md)|[if](if-else.md)|[implicit](../operators/user-defined-conversion-operators.md)|  
|[in](in.md)|[int](../builtin-types/integral-numeric-types.md)|[interface](interface.md)|[internal](internal.md)|
|[is](is.md)|[lock](lock-statement.md)|[long](../builtin-types/integral-numeric-types.md)|[namespace](namespace.md)|
|[new](../operators/new-operator.md)|[null](null.md)|[object](../builtin-types/reference-types.md)|[operator](../operators/operator-overloading.md)|
|[out](out.md)|[override](override.md)|[params](params.md)|[private](private.md)|
|[protected](protected.md)|[public](public.md)|[readonly](readonly.md)|[record](../../programming-guide/classes-and-structs/records.md)|
|[ref](ref.md)|[return](return.md)|[sbyte](../builtin-types/integral-numeric-types.md)|[sealed](sealed.md)|
|[short](../builtin-types/integral-numeric-types.md)|[sizeof](../operators/sizeof.md)|[stackalloc](../operators/stackalloc.md)|[static](static.md)|
|[string](../builtin-types/reference-types.md)|[struct](../builtin-types/struct.md)|[switch](switch.md)|[this](this.md)|
|[throw](throw.md)|[true](../builtin-types/bool.md)|[try](try-catch.md)|[typeof](../operators/type-testing-and-cast.md#typeof-operator)|
|[uint](../builtin-types/integral-numeric-types.md)|[ulong](../builtin-types/integral-numeric-types.md)|[unchecked](unchecked.md)|[unsafe](unsafe.md)|
|[ushort](../builtin-types/integral-numeric-types.md)|[using](using.md)|[virtual](virtual.md)|[void](../builtin-types/void.md)|
|[volatile](volatile.md)|[while](while.md)|

## <a name="contextual-keywords"></a>コンテキスト キーワード

 コンテキスト キーワードを使用して、コード内で特定の意味を付与することができます。ただし C# ではコンテキスト キーワードは予約語ではありません。 `partial` や `where` など、2 つ以上のコンテキストで特別な意味を持つコンテキスト キーワードもあります。  
  
||||  
|---|---|---|  
|[add](add.md)|[alias](extern-alias.md)|[ascending](ascending.md)|
|[async](async.md)|[await](../operators/await.md)|[by](by.md)|
|[descending](descending.md)|[dynamic](../builtin-types/reference-types.md)|[equals](equals.md)|
|[from](from-clause.md)|[get](get.md)|[global](../operators/namespace-alias-qualifier.md)|
|[group](group-clause.md)|[init](init.md)|[into](into.md)|
|[join](join-clause.md)|[let](let-clause.md)|[nameof](../operators/nameof.md)|
|[nint](../builtin-types/nint-nuint.md)|[notnull](../../programming-guide/generics/constraints-on-type-parameters.md#notnull-constraint)|[nuint](../builtin-types/nint-nuint.md)|
|[on](on.md)|[orderby](orderby-clause.md)|[partial (型)](partial-type.md)|
|[partial (メソッド)](partial-method.md)|[remove](remove.md)|[select](select-clause.md)|
|[set](set.md)|[unmanaged (ジェネリック型制約)](../../programming-guide/generics/constraints-on-type-parameters.md#unmanaged-constraint)|[value](value.md)|
|[var](var.md)|[when (フィルター条件)](when.md)|[where (ジェネリック型制約)](where-generic-type-constraint.md)|
|[where (クエリ句)](where-clause.md)|[with](../operators/with-expression.md)|[yield](yield.md)||

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
