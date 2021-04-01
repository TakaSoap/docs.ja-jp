---
title: 'サンプル XML ファイル: 一般的な購買発注書 - LINQ to XML'
description: (例で使用されている) この XML ファイルには、購買発注書に関するデータが含まれています。
ms.date: 07/20/2015
ms.assetid: dcbfb859-24fc-4758-b01c-51d1b6f644e6
ms.openlocfilehash: a54faad471459ac981ebe5fda0be4c1443cb183b
ms.sourcegitcommit: 0c3ce6d2e7586d925a30f231f32046b7b3934acb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/08/2020
ms.locfileid: "103412100"
---
# <a name="sample-xml-file-typical-purchase-order-linq-to-xml"></a>サンプル XML ファイル: 一般的な購買発注書 (LINQ to XML)

次の XML ファイルは、LINQ to XML ドキュメントのさまざまな例で使用されます。 このファイルは、一般的な購買発注書です。

## <a name="purchaseorderxml"></a>PurchaseOrder.xml

```xml
<?xml version="1.0"?>
<PurchaseOrder PurchaseOrderNumber="99503" OrderDate="1999-10-20">
  <Address Type="Shipping">
    <Name>Ellen Adams</Name>
    <Street>123 Maple Street</Street>
    <City>Mill Valley</City>
    <State>CA</State>
    <Zip>10999</Zip>
    <Country>USA</Country>
  </Address>
  <Address Type="Billing">
    <Name>Tai Yee</Name>
    <Street>8 Oak Avenue</Street>
    <City>Old Town</City>
    <State>PA</State>
    <Zip>95819</Zip>
    <Country>USA</Country>
  </Address>
  <DeliveryNotes>Please leave packages in shed by driveway.</DeliveryNotes>
  <Items>
    <Item PartNumber="872-AA">
      <ProductName>Lawnmower</ProductName>
      <Quantity>1</Quantity>
      <USPrice>148.95</USPrice>
      <Comment>Confirm this is electric</Comment>
    </Item>
    <Item PartNumber="926-AA">
      <ProductName>Baby Monitor</ProductName>
      <Quantity>2</Quantity>
      <USPrice>39.98</USPrice>
      <ShipDate>1999-05-21</ShipDate>
    </Item>
  </Items>
</PurchaseOrder>
```