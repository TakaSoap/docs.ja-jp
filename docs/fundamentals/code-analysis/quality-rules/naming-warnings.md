---
title: 名前付け規則 (コード分析)
description: コード分析の名前付け規則について説明します。
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.namingrules
helpviewer_keywords:
- managed code analysis rules, naming rules
- naming rules
- rules, naming
author: gewarren
ms.author: gewarren
ms.openlocfilehash: 88e7ec6bc1051fa98c46696b2193a5d5912eb111
ms.sourcegitcommit: 2e4adc490c1d2a705a0592b295d606b10b9f51f1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "96591193"
---
# <a name="naming-rules"></a>名前付け規則

名前付け規則は、.NET デザインガイドラインの名前付け規則への準拠をサポートします。

## <a name="in-this-section"></a>このセクションの内容

|ルール|説明|
|----------|-----------------|
|[CA1700:列挙型値に 'Reserved' という名前を指定しません](ca1700.md)|この規則では、"reserved" を含む名前の列挙体のメンバーは、現在使用されていなくても、将来的なバージョンでは名前を変更するか削除されるプレースホルダーと想定しています。 メンバーの名前変更や削除は、互換性に影響する変更点です。|
|[CA1707:識別子はアンダースコアを含むことはできません](ca1707.md)|名前付け規則では、識別子名にアンダースコア (_) 文字を含めることができません。 この規則により、名前空間、型、メンバー、およびパラメーターがチェックされます。|
|[CA1708:識別子は、大文字と小文字の区別以外にも相違していなければなりません](ca1708.md)|名前空間、型、メンバー、およびパラメーターの各識別子は、大文字/小文字以外のみでは区別できません。共通言語ランタイムを対象とする言語は、大文字と小文字を区別する必要はないためです。|
|[CA1710:識別子は、正しいサフィックスを含んでいなければなりません](ca1710.md)|慣例として、特定の基本型を拡張する型、特定のインターフェイスを実装する型、またはこれらの型から派生した型の名前には、基本型またはインターフェイスに関連付けられたサフィックスがあります。|
|[CA1711:識別子は、不適切なサフィックスを含むことはできません](ca1711.md)|規則では、特定の基本型を拡張する型、特定のインターフェイスを実装する型、またはそのような型から派生した型の名前にのみ、固有の予約済みサフィックスを末尾に付けます。 その他の型名では、予約済みのサフィックスを使用しないでください。|
|[CA1712:列挙型値を型名のプレフィックスにしません](ca1712.md)|型情報は開発ツールによって提供されることが予想されるため、列挙型メンバーの名前の前には型名が付けられません。|
|[CA1713:イベントは、before または after プレフィックスを含むことはできません](ca1713.md)|イベント名が "Before" または "After" で始まっています。 特定のシーケンスで発生する関連イベントに名前を付ける場合、現在時制または過去時制を使用して、アクション シーケンスの相対的な位置を示します。|
|[CA1714:フラグ列挙型は、複数形の名前を含んでいなければなりません](ca1714.md)|パブリック列挙型には FlagsAttribute 属性があり、その名前は "s" で終了しません。 FlagsAttribute でマークされた型には複数形の名前が付いています。これは、属性が複数の値を指定できることを示すためです。|
|[CA1715:識別子は正しいプレフィックスを含んでいなければなりません](ca1715.md)|外部から参照できるインターフェイスの名前が、大文字の "I" で始まっていません。  外部から参照できる型またはメソッドのジェネリック型パラメーターの名前が、大文字の "T" で始まっていません。|
|[CA1716:識別子はキーワードと同一にすることはできません](ca1716.md)|名前空間の名前または型の名前が、プログラミング言語で、予約済みのキーワードと一致します。 名前空間と型の識別子は、共通言語ランタイムを対象にする言語で定義されているキーワードと一致しないようにします。|
|[CA1717:FlagsAttribute 列挙型のみが複数形の名前を含んでいなければなりません](ca1717.md)|名前付け規則では、列挙体の複数形の名前は同時に複数の列挙値を指定できることを意味します。|
|[CA1720:識別子には型名を含めないでください](ca1720.md)|外部から参照できるメンバーのパラメーター名にデータ型の名前が含まれているか、外部から参照できるメンバーの名前に言語固有のデータ型の名前が含まれています。|
|[CA1721:プロパティ名は get メソッドと同一にすることはできません](ca1721.md)|パブリック メンバーまたはプロテクト メンバーの名前が、"Get" から始まっているか、パブリック プロパティまたはプロテクト プロパティの名前と一致します。 "Get" メソッドとプロパティには、それぞれの機能を明確に区別する名前を指定しなければなりません。|
|[CA1724:型名は名前空間と同一にすることはできません](ca1724.md)|型名は、.NET 名前空間の名前と同じにすることはできません。 この規則に違反すると、ライブラリの操作性が低下する可能性があります。|
|[CA1725:パラメーター名は基本宣言と同一でなければなりません](ca1725.md)|オーバーライド階層のパラメーターに対する一貫性のある名前付けによって、メソッド オーバーライドの有用性が高まります。 派生メソッドのパラメーター名が基本宣言のパラメーター名と異なる場合、メソッドが基本メソッドのオーバーライドであるか、またはメソッドの新しいオーバーライドであるかについて混乱が生じる可能性があります。|