---
title: 快速入门：必应拼写检查 API
titlesuffix: Azure Cognitive Services
description: 展示了如何开始使用必应拼写检查 API。
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 4af90c0e1ffd6dbadcb31eb6141bc24f746d01f9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316401"
---
# <a name="quickstart-your-first-spell-check-request"></a>快速入门：第一个拼写检查请求

在“搜索”下获取[认知服务访问密钥](https://azure.microsoft.com/try/cognitive-services/)。  另请参阅[认知服务定价 - 必应搜索 API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)。

要检查文本字符串是否存在拼写和语法错误，你需要向以下终结点发送 GET 请求：  
  
```
https://api.cognitive.microsoft.com/bing/v5.0/spellcheck
```

> [!NOTE]
> V7 预览终结点：
> 
> ```
> https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
> ```  
  
请求必须使用 HTTPS 协议。

我们建议所有请求都源自一个服务器。 将密钥作为客户端应用程序的一部分进行分发会为恶意第三方提供更多访问它的机会。 另外，从服务器发出调用可以为未来版本的 API 提供一个单一升级点。

请求必须指定 [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#text) 查询参数，其中包含要检查的文本字符串。 尽管是可选的，但请求还应该指定 [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#mkt) 查询参数，它确定你希望结果源自其中的市场。 对于诸如 `mode` 这样的可选查询参数列表，请参阅[查询参数](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#query-parameters)。 所有查询参数值都必须是 URL 编码。  
  
请求必须指定 [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#subscriptionkey) 标头。 尽管可视需要添加，但仍建议还指定以下请求头：  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#location)  

对于所有请求和响应头列表，请参阅[标头](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#headers)。

## <a name="the-request"></a>请求

以下显示了包含所有建议查询参数和标头的请求。 如果是第一次调用任何必应 API，请勿包括客户端 ID 标头。 只有在以前调用过必应 API 且必应针对用户和设备组合返回了客户端 ID 的情况下，才包括客户端 ID。 
  
```  
GET https://api.cognitive.microsoft.com/bing/v5.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> V7 预览请求：

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

以下显示了对之前请求的响应。 此示例还展示了必应专用响应头。

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  


## <a name="next-steps"></a>后续步骤

试用 API。 转到[拼写检查 API 测试控制台](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358)。 

有关使用响应对象的详细信息，请参阅[拼写检查文本字符串](./proof-text.md)。

