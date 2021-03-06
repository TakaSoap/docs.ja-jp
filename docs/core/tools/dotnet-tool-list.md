---
title: dotnet tool list コマンド
description: dotnet tool list コマンドでは、お使いのコンピューターにインストールされている .NET Core ツールの一覧を表示します。
ms.date: 02/14/2020
ms.openlocfilehash: 28f9155407d1238f8b0960b69b34ea329ca0e8e6
ms.sourcegitcommit: 927b7ea6b2ea5a440c8f23e3e66503152eb85591
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2020
ms.locfileid: "81463344"
---
# <a name="dotnet-tool-list"></a>dotnet tool list

**この記事の対象:** ✔️ .NET Core 2.1 SDK 以降のバージョン

## <a name="name"></a>名前

`dotnet tool list` - お使いのコンピューター上に現在インストールされている指定した種類のすべての [.NET Core ツール](global-tools.md)を一覧表示します。

## <a name="synopsis"></a>構文

```dotnetcli
dotnet tool list -g|--global

dotnet tool list --tool-path <PATH>

dotnet tool list

dotnet tool list -h|--help
```

## <a name="description"></a>説明

`dotnet tool list` コマンドでは、お使いのコンピューター上にインストールされている .NET Core グローバル、ツールパス、またはローカルのすべてのツールを一覧表示する方法を提供します。 コマンドでは、パッケージ名、インストールされているバージョン、およびツール コマンドを一覧表示します。  コマンドを使用するには、次のいずれかを指定します。

* 既定の場所にインストールされているグローバル ツール。 `--global` オプションを使用します
* カスタムの場所にインストールされているグローバル ツール。 `--tool-path` オプションを使用します。
* ローカル ツール。 `--global` および `--tool-path` オプションを省略します。

**ローカル ツールは .NET Core SDK 3.0 以降で使用できます。**

## <a name="options"></a>オプション

- **`-g|--global`**

  ユーザー全体のグローバル ツールを一覧表示します。 `--tool-path` オプションと組み合わせることはできません。 `--global` と `--tool-path` の両方を省略すると、ローカル ツールが一覧表示されます。

- **`-h|--help`**

  コマンドの短いヘルプを印刷します。

- **`--tool-path <PATH>`**

  グローバル ツールを検索するカスタムの場所を指定します。 PATH は絶対パスでも相対パスでもかまいません。 `--global` オプションと組み合わせることはできません。 `--global` と `--tool-path` の両方を省略すると、ローカル ツールが一覧表示されます。

## <a name="examples"></a>使用例

- **`dotnet tool list -g`**

  お使いのコンピューター (現在のユーザー プロファイル) 上でユーザー全体にインストールされているすべてのグローバル ツールを一覧表示します。

- **`dotnet tool list --tool-path c:\global-tools`**

  特定の Windows ディレクトリからグローバル ツールを一覧表示します。

- **`dotnet tool list --tool-path ~/bin`**

  特定の Linux/macOS ディレクトリからグローバル ツールを一覧表示します。

- **`dotnet tool list`**

  現在のディレクトリで使用可能なすべてのローカル ツールを一覧表示します。

## <a name="see-also"></a>関連項目

- [.NET Core ツール](global-tools.md)
- [チュートリアル: .NET Core CLI を使って .NET Core グローバル ツールをインストールして使用する](global-tools-how-to-use.md)
- [チュートリアル: .NET Core CLI を使って .NET Core ローカル ツールをインストールして使用する](local-tools-how-to-use.md)
