---
title: 必应实体搜索终结点
titlesuffix: Azure Cognitive Services
description: 实体搜索 API 终结点摘要。
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: aahi
ms.openlocfilehash: 578791c8beb34b08b28037977147d41db83f4a4d
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164908"
---
# <a name="entity-search-endpoints"></a>实体搜索终结点
实体搜索 API 包括一个终结点。

## <a name="endpoint"></a>终结点
要请求实体搜索结果，请将请求发送到以下终结点。 使用标头和 URL 参数来定义更多规范。

终结点 `GET`： 
``` 
https://api.cognitive.microsoft.com/bing/v7.0/entities
```

需要以下 URL 参数：
- mkt。 产生结果的市场。 
- q。 实体搜索查询。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [必应实体搜索快速入门](quickstarts/csharp.md)

## <a name="see-also"></a>另请参阅 

[必应实体搜索概述](search-the-web.md )
[API 参考](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)
