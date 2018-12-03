---
title: 快速入门：使用 Python 执行搜索 - 必应 Web 搜索 API
titleSuffix: Azure Cognitive Services
description: 本快速入门介绍了如何使用 Python 进行你的第一次必应 Web 搜索 API 调用并接收 JSON 响应。
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 8/16/2018
ms.author: aahi
ms.openlocfilehash: 0f6f3991e01e4eb6919d958002ef6230a2570dbe
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52309447"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>快速入门：使用 Python 调用必应 Web 搜索 API  
在“搜索”下获取[认知服务访问密钥](https://azure.microsoft.com/try/cognitive-services/)。  另请参阅[认知服务定价 - 必应搜索 API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)。

使用本快速入门在不到 10 分钟时间内进行你的第一次必应 Web 搜索 API 调用并接收 JSON 响应。  

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

另请参阅[认知服务定价 - 必应搜索 API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)。

此示例作为 Jupyter 笔记本在 [MyBinder](https://mybinder.org) 上运行。 单击“启动活页夹”提示标记：

[![活页夹](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="define-variables"></a>定义变量

将 `subscription_key` 值替换为来自你的 Azure 帐户的有效订阅密钥。

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

声明必应 Web 搜索 API 终结点。 如果遇到任何授权错误，请在 Azure 仪表板中针对必应搜索终结点仔细检查此值。

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

可以通过替换 `search_term` 的值随意自定义搜索查询。

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>发出请求

以下代码块使用 `requests` 库调用必应 Web 搜索 API 并将结果作为 JSON 对象返回。 API 密钥是在 `headers` 字典中传入的，搜索词和查询参数是在 `params` 字典中传入的。 有关选项和参数的完整列表，请参阅[必应 Web 搜索 API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) 文档。

```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "textDecorations":True, "textFormat":"HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>格式化并显示响应

`search_results` 对象包括搜索结果以及元数据，例如相关查询和页面。 此代码使用 `IPython.display` 库格式化响应并在浏览器中显示响应。

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"],v["name"],v["snippet"]) \
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Github 上的示例代码

如果希望在本地运行此代码，[GitHub 上提供了完整的示例](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingWebSearchv7.js)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [必应 Web 搜索单页应用教程](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
