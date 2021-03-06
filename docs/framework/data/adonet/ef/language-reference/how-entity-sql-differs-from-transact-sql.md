---
title: Entity SQL と Transact-SQL の相違点
ms.date: 03/30/2017
ms.assetid: 9c9ee36d-f294-4c8b-a196-f0114c94f559
ms.openlocfilehash: 299cb9bbe90c72619cf809a8fc673fca456bd6fd
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79150232"
---
# <a name="how-entity-sql-differs-from-transact-sql"></a>Entity SQL と Transact-SQL の相違点
このトピックでは、Transact-SQL との違[!INCLUDE[esql](../../../../../../includes/esql-md.md)]いについて説明します。  
  
## <a name="inheritance-and-relationships-support"></a>継承とリレーションシップのサポート  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]概念エンティティ スキーマと直接連携し、継承やリレーションシップなどの概念モデル機能をサポートします。  
  
 継承を操作するときは、スーパータイプ インスタンスのコレクションからサブタイプのインスタンスを選択すると便利である場合がよくあります。 の[型](oftype-entity-sql.md)演算子[!INCLUDE[esql](../../../../../../includes/esql-md.md)](C#`oftype`シーケンスに似ています) は、この機能を提供します。  
  
## <a name="support-for-collections"></a>コレクションのサポート  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]コレクションは、一流のエンティティとして扱われます。 次に例を示します。  
  
- コレクションの式は、`from` 句内で有効です。  
  
- `in` サブクエリと `exists` サブクエリは任意のコレクションを使用できるように一般化されています。  
  
     サブクエリは一種のコレクションです。 `e1 in e2` および `exists(e)` は、これらの演算を実行する [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 構造です。  
  
- `union`、`intersect`、`except` などの集合演算は、現在ではコレクションに対して実行されます。  
  
- 結合はコレクションに対して実行されます。  
  
## <a name="support-for-expressions"></a>式のサポート  
 Transact-SQL には、サブクエリ (テーブル) と式 (行と列) があります。  
  
 コレクションと入れ子になったコレクションをサポートするには[!INCLUDE[esql](../../../../../../includes/esql-md.md)]、すべての式を作成します。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]は Transact-SQL よりもコンポーザブルであり、すべての式をどこでも使用できます。 クエリ式は常に投射型のコレクションとなり、コレクション式が許可されている任意の場所で使用できます。 で[!INCLUDE[esql](../../../../../../includes/esql-md.md)]サポートされていない Transact-SQL 式の詳細については、「[サポートされていない式](unsupported-expressions-entity-sql.md)」を参照してください。  
  
 次の [!INCLUDE[esql](../../../../../../includes/esql-md.md)] クエリはすべて有効です。  
  
```sql  
1+2 *3  
"abc"  
row(1 as a, 2 as b)  
{ 1, 3, 5}
e1 union all e2  
set(e1)  
```  
  
## <a name="uniform-treatment-of-subqueries"></a>サブクエリの一貫した処理  
 テーブルに重点を置いて、Transact-SQL はサブクエリのコンテキスト解釈を実行します。 たとえば、`from`句のサブクエリはマルチセット (テーブル) と見なされます。 ただし、`select` 句で使用される同じサブクエリは、スカラー サブクエリと見なされます。 同様に、`in`演算子の左側で使用されるサブクエリはスカラー サブクエリと見なされ、右側はマルチセット サブクエリであると想定されます。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] ではこれらの違いが排除されます。 式は、使用するコンテキストに依存しない、一貫した方法で解釈されます。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]すべてのサブクエリがマルチセットサブクエリであると見なされます。 サブクエリからスカラー値が必要な場合は、[!INCLUDE[esql](../../../../../../includes/esql-md.md)]コレクション`anyelement`(この場合はサブクエリ) を操作する演算子を提供し、コレクションからシングルトン値を抽出します。  
  
### <a name="avoiding-implicit-coercions-for-subqueries"></a>サブクエリの暗黙の強制型変換の回避  
 サブクエリの一貫した処理に伴う二次的作用として、スカラー値へのサブクエリの暗黙的な変換があります。 具体的には、Transact-SQL では、複数の行のマルチセット (フィールドが 1 つ) は、データ型がフィールドのスカラー値に暗黙的に変換されます。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では、この暗黙の強制型変換をサポートしていません。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では、コレクションからシングルトン値を抽出するための ANYELEMENT 演算子と、クエリ式の実行中に row ラッパーの作成を回避するための `select value` 句が提供されています。  
  
## <a name="select-value-avoiding-the-implicit-row-wrapper"></a>SELECT VALUE: 暗黙の row ラッパーの回避  
 Transact-SQL サブクエリの select 句は、句内の項目を暗黙的にラップする行を作成します。 これは、スカラーやオブジェクトのコレクションを作成できないことを意味します。 Transact-SQL では、1 つのフィールドを持つ行の型と、同じデータ型のシングルトン値の間で暗黙的な強制変換が可能です。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] には、暗黙の行の構築をスキップする `select value` 句が用意されています。 `select value` 句には 1 つの項目のみを指定できます。 このような句を使用した場合、`select` 句内の項目には row ラッパーは構築されず、目的の構造を持つコレクションを作成できます。たとえば、`select value a` のように指定します。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] には、任意の行を構築するための行コンストラクターも用意されています。 `select` は、投影の 1 つ以上の要素を受け取り、結果はフィールドを持つデータ レコードになります。次のように指定します。  
  
 `select a, b, c`  
  
## <a name="left-correlation-and-aliasing"></a>左の相関関係と別名定義  
 Transact-SQL では、指定されたスコープ内の式 (または`select``from`のような単一の句) は、同じスコープ内で前に定義された式を参照できません。 SQL の一部の方言 (Transact-SQL を含む) は、`from`この節でこれらの制限された形式をサポートしています。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]節内の左の相関を`from`一般化し、それらを均一に扱います。 `from` 句内の式は、追加の構文を使用せずに、同じ句内で先に作成された定義 (左側の定義) を参照できます。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では、`group by` 句を伴うクエリにも制限を課しています。 このようなクエリの`select`句および`having`句の式は、エイリアスを`group by`介してのみキーを参照できます。 次のコンストラクトは Transact-SQL で有効[!INCLUDE[esql](../../../../../../includes/esql-md.md)]ですが、 には含まれていません。  
  
```sql  
SELECT t.x + t.y FROM T AS t group BY t.x + t.y
```  
  
 これを [!INCLUDE[esql](../../../../../../includes/esql-md.md)] で実行するには、次のように指定します。  
  
```sql  
SELET k FROM T AS t GROUP BY (t.x + t.y) AS k
```  
  
## <a name="referencing-columns-properties-of-tables-collections"></a>テーブル (コレクション) の列 (プロパティ) の参照  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 内の列の参照は、すべてテーブルの別名を使用して修飾する必要があります。 次のコンストラクト`a`( テーブル`T`の有効な列 ) は Transact-SQL[!INCLUDE[esql](../../../../../../includes/esql-md.md)]では有効ですが、 では有効ではありません。  
  
```sql  
SELECT a FROM T
```  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] の形式は次のとおりです。  
  
```sql  
SELECT t.a AS A FROM T AS t
```  
  
 テーブルの別名は `from` 句では省略できます。 テーブル名は暗黙的な別名として使用されます。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では次の形式も使用できます。  
  
```sql  
SELET Tab.a FROM Tab
```  
  
## <a name="navigation-through-objects"></a>オブジェクト間の移動  
 Transact-SQL では、テーブルの列 (行) を参照するために "." 表記が使用されます。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] ではこの表記法を拡張し (プログラミング言語から借用)、オブジェクトのプロパティ間の移動をサポートしています。  
  
 たとえば、`p` が Person 型の式である場合、この人の住所の市区町村を参照するには次の [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 構文が使用されます。  
  
```sql  
p.Address.City
```  
  
## <a name="no-support-for-"></a>* のサポートなし  
 Transact-SQL では、行全体の別名として非修飾 * 構文、そのテーブル\*のフィールドのショートカット\*として修飾構文 (t. ) がサポートされています。 さらに、Transact-SQL では、null を含\*む特殊なカウント( ) 集計が可能です。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では、* 構造をサポートしていません。 フォーム`select * from T`の Transact-SQL クエリを`select T1.* from T1, T2...`、`select value t from T as t`および`select value t1 from T1 as t1, T2 as t2...`[!INCLUDE[esql](../../../../../../includes/esql-md.md)]でそれぞれ で表すことができます。 また、これらの構造は継承 (値の置換可能性) に対応していますが、`select *` Variant 型では宣言された型の最上位レベルのプロパティに限定されています。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] は `count(*)` 集計をサポートしていません。 代わりに `count(0)` を使用してください  
  
## <a name="changes-to-group-by"></a>Group By への変更  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では `group by` キーの別名定義をサポートしています。 句および句`select`の式`having`は、これらのエイリアスを`group by`使用してキーを参照する必要があります。 たとえば、次のような [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 構文があるとします。  
  
```sql  
SELECT k1, count(t.a), sum(t.a)
FROM T AS t
GROUP BY t.b + t.c AS k1
```  
  
 ...は、次の Transact-SQL と同じです。  
  
```sql  
SELECT b + c, count(*), sum(a)
FROM T
GROUP BY b + c
```  
  
## <a name="collection-based-aggregates"></a>コレクションベースの集計  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] は、2 種類の集計をサポートしています。  
  
 コレクションベースの集計は、コレクションに対して演算を行い、集計結果を生成します。 これらはクエリ内の任意の場所で使用でき、`group by` 句を必要としません。 次に例を示します。  
  
```sql  
SELECT t.a AS a, count({1,2,3}) AS b FROM T AS t
```  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] は、SQL スタイルの集計もサポートしています。 次に例を示します。  
  
```sql  
SELECT a, sum(t.b) FROM T AS t GROUP BY t.a AS a
```  
  
## <a name="order-by-clause-usage"></a>ORDER BY 句の使用法  
Transact-SQL`ORDER BY`では、句を最上位`SELECT .. FROM .. WHERE`ブロックでのみ指定できます。 入[!INCLUDE[esql](../../../../../../includes/esql-md.md)]れ子になった`ORDER BY`式を使用してクエリ内の任意の場所に配置できますが、入れ子になったクエリでの順序は保持されません。  
  
```sql  
-- The following query will order the results by the last name  
SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorks.Contact AS C1
        ORDER BY C1.LastName  
```  
  
```sql  
-- In the following query ordering of the nested query is ignored.  
SELECT C2.FirstName, C2.LastName  
    FROM (SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorks.Contact as C1  
        ORDER BY C1.LastName) as C2  
```  
  
## <a name="identifiers"></a>識別子  
 Transact-SQL では、識別子の比較は現在のデータベースの照合順序に基づいています。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] の識別子では、常に大文字と小文字は区別されず、アクセントは区別されます (つまり、[!INCLUDE[esql](../../../../../../includes/esql-md.md)] ではアクセントのある文字とアクセントのない文字が区別されます。たとえば、'a' と 'ấ' は等しくありません)。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] は、同じように表示されても別のコード ページに由来する文字の複数のバージョンを別々の文字として扱います。 詳細については、「[入力文字セット](input-character-set-entity-sql.md)」を参照してください。  
  
## <a name="transact-sql-functionality-not-available-in-entity-sql"></a>Entity SQL では使用できない Transact-SQL 機能  
 次の Transact-SQL 機能は、[!INCLUDE[esql](../../../../../../includes/esql-md.md)]では使用できません。  
  
 DML  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]現在、DML ステートメント (挿入、更新、削除) はサポートされません。  
  
 DDL (DDL)  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] の現在のバージョンでは DDL はサポートされていません。  
  
 命令型プログラミング  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]は、Transact-SQL とは異なり、命令型プログラミングのサポートを提供しません。 代わりにプログラミング言語を使用します。  
  
 グループ化関数  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] ではグループ化関数 (CUBE, ROLLUP、GROUPING_SET など) はサポートしていません。  
  
 分析関数  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では分析関数はまだサポートしていません。  
  
 組み込み関数、演算子  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]は、Transact-SQL の組み込み関数と演算子のサブセットをサポートします。 これらの演算子と関数の多くは、主要なストア プロバイダーによりサポートされています。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]は、プロバイダー マニフェストで宣言されたストア固有の関数を使用します。 さらに、Entity Framework を使用して、組み込みおよびユーザー定義の既存のストア関数[!INCLUDE[esql](../../../../../../includes/esql-md.md)]を宣言して使用できます。  
  
 [ヒント]  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] ではクエリ ヒントのメカニズムは提供していません。  
  
 クエリ結果のバッチ処理  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] では、クエリ結果のバッチ処理はサポートされていません。 たとえば、次の例は有効な Transact-SQL (バッチとして送信) です。  
  
```sql  
SELECT * FROM products;
SELECT * FROM catagories;
```  
  
 ただし、同等の [!INCLUDE[esql](../../../../../../includes/esql-md.md)] はサポートされていません。  
  
```sql  
SELECT value p FROM Products AS p;
SELECT value c FROM Categories AS c;
```  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] は、コマンドごとに 1 つの結果生成クエリ ステートメントのみをサポートします。  
  
## <a name="see-also"></a>関連項目

- [Entity SQL の概要](entity-sql-overview.md)
- [サポートされていない式](unsupported-expressions-entity-sql.md)
