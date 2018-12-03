---
title: 快速入门：创建视觉搜索查询 (Node.js) - 必应视觉搜索
titleSuffix: Azure Cognitive Services
description: 如何将图像上传到必应视觉搜索 API 并取回有关该图像的见解。
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 553d068d70f7e722f3c8e4de3978f3583b941963
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442517"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-javascript"></a>快速入门：使用 JavaScript 编写的第一个必应视觉搜索查询

必应视觉搜索 API 将返回有关所提供的图像的信息。 可以通过使用图像的 URL、见解标记或通过上传图像来提供图像。 有关这些选项的信息，请参阅[什么是必应视觉搜索 API？](../overview.md) 本文演示了如何上传图像。 上传图像在移动方案中非常有用，在这类方案中，你可以拍摄知名地标的照片并获取有关它的信息。 例如，见解可能包括有关地标的花边消息。 

如果上传本地图像，则下面显示了必须在 POST 的正文中包含的窗体数据。 窗体数据必须包含 Content-Disposition 标头。 它的 `name` 参数必须设置为“image”，`filename` 参数可以设置为任何字符串。 窗体内容是图像的二进制文件。 可以上传的最大图像大小为 1 MB。 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

本文包括一个简单的控制台应用程序，它发送必应视觉搜索 API 请求并显示 JSON 搜索结果。 虽然此应用程序是用 JavaScript 编写的，但 API 是一种 RESTful Web 服务，与任何可以发出 HTTP 请求并解析 JSON 的编程语言兼容。 

## <a name="prerequisites"></a>先决条件
对于本快速入门，你需要以 S9 价格层开始订阅，如[认知服务定价 - 必应搜索 API](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/) 中所示。 

若要在 Azure 门户中开始订阅，请执行以下操作：
1. 在 Azure 门户顶部显示 `Search resources, services, and docs` 的文本框中输入“BingSearchV7”。  
2. 在“市场”下的下拉列表中，选择 `Bing Search v7`。
3. 输入新资源的 `Name`。
4. 选择 `Pay-As-You-Go` 订阅。
5. 选择 `S9` 定价层。
6. 单击 `Enable` 即可开始订阅。

需要使用 [Node.js 6](https://nodejs.org/en/download/) 来运行此代码。

## <a name="running-the-application"></a>运行应用程序

以下代码演示如何使用 Node.js 中的 FormData 发送消息。

若要运行此应用程序，请执行以下步骤：

1. 为项目创建一个文件夹（或使用你最喜欢的 IDE 或编辑器）。
2. 从命令提示符或终端导航至刚刚创建的文件夹。
3. 安装请求模块：  
  ```  
  npm install request  
  ```  
3. 安装窗体数据模块：  
  ```  
  npm install form-data  
  ```  
4. 创建一个名为 GetVisualInsights.js 的文件，并将以下代码添加到其中。
5. 将 `subscriptionKey` 值替换为订阅密钥。
6. 将 `imagePath` 值替换为要上传的图像的路径。
7. 运行该程序。  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>后续步骤

[使用见解标记获取有关图像的见解](../use-insights-token.md)  
[必应视觉搜索图像上传教程](../tutorial-visual-search-image-upload.md)
[必应视觉搜索单页应用教程](../tutorial-bing-visual-search-single-page-app.md)  
[必应视觉搜索概述](../overview.md)  
[试试看](https://aka.ms/bingvisualsearchtryforfree)  
[获取免费试用版访问密钥](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[必应视觉搜索 API 参考](https://aka.ms/bingvisualsearchreferencedoc)
