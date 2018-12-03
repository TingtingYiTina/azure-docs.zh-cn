---
title: LUIS 预构建实体货币参考 - Azure | Microsoft Docs
titleSuffix: Azure
description: 本文包含了语言理解 (LUIS) 中的货币预构建实体信息。
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 6346ce4e3c9e51852a9a98904b797b6eb64970e4
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334000"
---
# <a name="currency-entity"></a>货币实体
预构建货币实体检测许多国家/地区中许多面额的货币，无论 LUIS 应用语言区域是怎样的。 此实体已定型，因此不需要将包含货币的陈述示例添加到应用程序意向中。 [许多语言区域](luis-reference-prebuilt-entities.md)都支持货币实体。 

## <a name="types-of-currency"></a>货币类型
货币托管在 [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) Github 存储库中

## <a name="resolution-for-currency-entity"></a>货币实体的解析
以下示例显示了 **builtin.currency** 实体的解析。

```JSON
{
  "query": "search for items under $10.99",
  "topScoringIntent": {
    "intent": "SearchForItems",
    "score": 0.926173568
  },
  "intents": [
    {
      "intent": "SearchForItems",
      "score": 0.926173568
    },
    {
      "intent": "None",
      "score": 0.07376878
    }
  ],
  "entities": [
    {
      "entity": "$10.99",
      "type": "builtin.currency",
      "startIndex": 23,
      "endIndex": 28,
      "resolution": {
        "unit": "Dollar",
        "value": "10.99"
      }
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤

了解 [datetimeV2](luis-reference-prebuilt-datetimev2.md)、[维度](luis-reference-prebuilt-dimension.md)和[电子邮件](luis-reference-prebuilt-email.md)实体。 