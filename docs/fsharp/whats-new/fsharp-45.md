---
title: F# 4.5 の新機能 - F# ガイド
description: F# 4.5 で利用可能な新機能の概要を確認します。
ms.date: 11/27/2019
ms.openlocfilehash: 560e3dd941f79b76d3b864ba0f6560be154ebc1a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79186136"
---
# <a name="whats-new-in-f-45"></a>F# 4.5 の新機能

F# 4.5 では、F# 言語に複数の改良が加えました。 これらの機能の多くは、F# で効率的なコードを記述できる一方で、このコードの安全性を確保するために組み合わせて追加されました。 この場合、これらの構成要素を使用する場合、言語にいくつかの概念を追加し、コンパイラ分析の量を大幅に増やします。

## <a name="get-started"></a>はじめに

F# 4.5 は、すべての .NET コアディストリビューションと Visual Studio ツールで使用できます。 [F# の使用を開始](../get-started/index.md)して、詳細を確認してください。

## <a name="span-and-byref-like-structs"></a>スパンとバイレフのような構造体

NET <xref:System.Span%601> Core で導入された型では、厳密に型指定された方法でメモリ内のバッファーを表すことができます。 次の例は、異なるバッファー表現を持つ 関数<xref:System.Span%601>を使用して再利用する方法を示しています。

```fsharp
let safeSum (bytes: Span<byte>) =
    let mutable sum = 0
    for i in 0 .. bytes.Length - 1 do
        sum <- sum + int bytes.[i]
    sum

// managed memory
let arrayMemory = Array.zeroCreate<byte>(100)
let arraySpan = new Span<byte>(arrayMemory)

safeSum(arraySpan) |> printfn "res = %d"

// native memory
let nativeMemory = Marshal.AllocHGlobal(100);
let nativeSpan = new Span<byte>(nativeMemory.ToPointer(), 100)

safeSum(nativeSpan) |> printfn "res = %d"
Marshal.FreeHGlobal(nativeMemory)

// stack memory
let mem = NativePtr.stackalloc<byte>(100)
let mem2 = mem |> NativePtr.toVoidPtr
let stackSpan = Span<byte>(mem2, 100)

safeSum(stackSpan) |> printfn "res = %d"
```

この重要な側面は、Span やその他[の byref のような構造体](../language-reference/structures.md#byreflike-structs)が、コンパイラによって非常に厳密な静的分析を実行し、予期しない方法で使用を制限することです。 これは、F# 4.5 で導入されたパフォーマンス、表現力、安全性の基本的なトレードオフです。

## <a name="revamped-byrefs"></a>改良されたバイレフ

F# 4.5 より前のバージョンでは、F# の[Byrefs](../language-reference/byrefs.md)は安全で、多くのアプリケーションでは不音でした。 byrefs に関するサウンドの問題は F# 4.5 で取り上げられており、span および byref のような構造体に対して行われたのと同じ静的分析も適用されました。

### <a name="inreft-and-outreft"></a>インレフ<>とアウトレフ<>

読み取り専用、書き込み専用、および読み取り/書き込みマネージ ポインターの概念`inref<'T>`を`outref<'T>`表すために、F# 4.5 では、それぞれ、読み取り専用ポインターと書き込み専用ポインターを表す型が導入されています。 それぞれに異なるセマンティクスがあります。 たとえば、`inref<'T>`に書き込むことはできません。

```fsharp
let f (dt: inref<DateTime>) =
    dt <- DateTime.Now // ERROR - cannot write to an inref!
```

既定では、何か変更可能として宣言されていない限り、型`inref<'T>`の推論は、F# コードの不変の性質に沿ったマネージ ポインターを推論します。 書き込み可能なものにするには、そのアドレスを操作する関数またはメンバー`mutable`に渡す前に、型を宣言する必要があります。 詳細については[、「Byrefs」](../language-reference/byrefs.md)を参照してください。

## <a name="readonly-structs"></a>読み取り専用構造体

F# 4.5 以降では、構造体<xref:System.Runtime.CompilerServices.IsReadOnlyAttribute>に次のようにアポイントを付けることができます。

```fsharp
[<IsReadOnly; Struct>]
type S(count1: int, count2: int) =
    member x.Count1 = count1
    member x.Count2 = count2
```

これにより、構造体で変更可能なメンバーを宣言することを禁止し、F# と C# がアセンブリから読み取り時に読み取り専用として扱うことを可能にするメタデータを出力します。 詳細については、「 [ReadOnly 構造体](../language-reference/structures.md#readonly-structs)」を参照してください。

## <a name="void-pointers"></a>ボイドポインタ

型`voidptr`は、次の関数と同様に、F# 4.5 に追加されます。

* `NativePtr.ofVoidPtr`void ポインタをネイティブの int ポインタに変換するには
* `NativePtr.toVoidPtr`ネイティブの int ポインターを void ポインターに変換するには

これは、void ポインターを使用するネイティブ コンポーネントと相互運用するときに役立ちます。

## <a name="the-match-keyword"></a>`match!` キーワード

この`match!`キーワードは、計算式の内部でパターン マッチングを強化します。

```fsharp
// Code that returns an asynchronous option
let checkBananaAsync (s: string) =
    async {
        if s = "banana" then
            return Some s
        else
            return None
    }

// Now you can use 'match!'
let funcWithString (s: string) =
    async {
        match! checkBananaAsync s with
        | Some bananaString -> printfn "It's banana!"
        | None -> printfn "%s" s
}
```

これにより、多くの場合、オプション (または他の型) と非同期などの計算式を混在させるコードを短くすることができます。 詳細については[、「match!」](../language-reference/computation-expressions.md#match)を参照してください。

## <a name="relaxed-upcasting-requirements-in-array-list-and-sequence-expressions"></a>配列、リスト、シーケンス式での緩和されたアップキャスト要件

配列、リスト、シーケンス式の別の内部から継承する型を混在させる場合は、従来、任意の派生型を親の型にアップキャストする`:>`必要`upcast`があります。 これは緩和され、次のように示されています。

```fsharp
let x0 : obj list  = [ "a" ] // ok pre-F# 4.5
let x1 : obj list  = [ "a"; "b" ] // ok pre-F# 4.5
let x2 : obj list  = [ yield "a" :> obj ] // ok pre-F# 4.5

let x3 : obj list  = [ yield "a" ] // Now ok for F# 4.5, and can replace x2
```

## <a name="indentation-relaxation-for-array-and-list-expressions"></a>配列およびリスト式のインデント緩和

F# 4.5 より前のバージョンでは、メソッド呼び出しに引数として渡された場合、配列とリスト式を過度にインデントする必要があった。 これは不要になりました。

```fsharp
module NoExcessiveIndenting =
    System.Console.WriteLine(format="{0}", arg = [|
        "hello"
    |])
    System.Console.WriteLine([|
        "hello"
    |])
```
