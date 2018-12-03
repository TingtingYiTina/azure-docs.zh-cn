---
title: 快速入门：获取句子长度，C# - 文本翻译 API
titleSuffix: Azure Cognitive Services
description: 本快速入门介绍如何使用 .NET Core 和文本翻译 API 来确定句子长度。
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 11/26/2018
ms.author: erhopf
ms.openlocfilehash: abdd87b9a86ed2482d5b53c10260fe28821b34ce
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333246"
---
# <a name="quickstart-get-sentence-lengths-with-the-translator-text-rest-api-c"></a>快速入门：使用文本翻译 REST API (C#) 获取句子长度

本快速入门介绍如何使用 .NET Core 和文本翻译 API 来确定句子长度。

此快速入门需要包含文本翻译资源的 [Azure 认知服务帐户](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)。 如果没有帐户，可以使用[免费试用版](https://azure.microsoft.com/try/cognitive-services/)获取订阅密钥。

## <a name="prerequisites"></a>先决条件

* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET NuGet 包](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)、[Visual Studio Code](https://code.visualstudio.com/download) 或你喜欢用的文本编辑器
* 语音服务的 Azure 订阅密钥

## <a name="create-a-net-core-project"></a>创建 .NET Core 项目

打开新的命令提示符（或终端会话）并运行以下命令：

```console
dotnet new console -o sentences-sample
cd sentences-sample
```

第一个命令执行两项操作。 它创建新的 .NET 控制台应用程序，并创建名为 `sentences-sample` 的目录。 第二个目录转到项目的目录。

接下来需安装 Json.Net。 在项目的目录中，运行以下命令：

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>将所需命名空间添加到项目

此前运行的 `dotnet new console` 命令创建了一个项目（包括 `Program.cs`）。 此文件是需放置应用程序代码的位置。 打开 `Program.cs`，替换现有的 using 语句。 这些语句可确保你有权访问生成并运行示例应用所需的所有类型。

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-determine-sentence-length"></a>创建一个用于确定句子长度的函数

在 `Program` 类中创建名为 `BreakSentence` 的函数。 该类封装用于调用 BreakSentence 资源的代码，并将结果输出到控制台。

```csharp
static void BreakSentence()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>设置订阅密钥、主机名称和路径

将以下行添加到 `BreakSentence` 函数。 你会注意到，除了 `api-version`，还可以定义输入语言。 在此示例中，语言为英语。

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/breaksentence?api-version=3.0&language=en";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

接下来需创建 JSON 对象并将其序列化，其中包含文本。 请记住，可以在 `body` 数组中传递多个对象。

```csharp
System.Object[] body = new System.Object[] { new { Text = @"How are you? I am fine. What did you do today?" } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>实例化客户端并发出请求

以下行实例化 `HttpClient` 和 `HttpRequestMessage`：

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>构造请求并输出响应

在 `HttpRequestMessage` 中，需执行以下操作：

* 声明 HTTP 方法
* 构造请求 URI
* 插入请求正文（序列化的 JSON 对象）
* 添加必需的标头
* 发出异步请求
* 输出响应

向 `HttpRequestMessage` 添加以下代码：

```csharp
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(jsonResponse);
Console.WriteLine("Press any key to continue.");
```

## <a name="put-it-all-together"></a>将其放在一起

最后一步是在 `Main` 函数中调用 `BreakSentence()`。 找到 `static void Main(string[] args)` 并添加以下行：

```csharp
BreakSentence();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>运行示例应用

上述操作完成后，就可以运行示例应用了。 从命令行（或终端会话）导航到项目目录，然后运行以下命令：

```console
dotnet run
```

## <a name="sample-response"></a>示例响应

```json
[
    {
        "sentLen": [
            13,
            11,
            22
        ]
    }
]
```

## <a name="clean-up-resources"></a>清理资源

请务必删除示例应用的源代码中的机密信息，例如订阅密钥。

## <a name="next-steps"></a>后续步骤

浏览此快速入门的示例代码和其他内容，包括音译和语言识别，以及 GitHub 上的其他示例文本翻译项目。

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 C# 示例](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>另请参阅

* [翻译文本](quickstart-csharp-translate.md)
* [直译文本](quickstart-csharp-transliterate.md)
* [按输入确定语言](quickstart-csharp-detect.md)
* [获取备用翻译](quickstart-csharp-dictionary.md)
* [获取支持的语言的列表](quickstart-csharp-languages.md)
* [根据输入确定句子长度](quickstart-csharp-sentences.md)
