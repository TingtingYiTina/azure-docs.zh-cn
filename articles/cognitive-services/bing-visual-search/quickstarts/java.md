---
title: 快速入门：创建视觉搜索查询 (Java) - 必应视觉搜索
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
ms.openlocfilehash: c1b63b12a48f5ccfb1a396ffa9282249b03893fe
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52445168"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-java"></a>快速入门：使用 Java 编写的第一个必应视觉搜索查询

必应视觉搜索 API 将返回有关所提供的图像的信息。 可以通过使用图像的 URL、见解标记或通过上传图像来提供图像。 有关这些选项的信息，请参阅[什么是必应视觉搜索 API？](../overview.md) 本文演示了如何上传图像。 上传图像在移动方案中非常有用，在这类方案中，你可以拍摄知名地标的照片并获取有关它的信息。 例如，见解可能包括有关地标的花边消息。 

如果上传本地图像，则下面显示了必须在 POST 的正文中包含的窗体数据。 窗体数据必须包含 Content-Disposition 标头。 它的 `name` 参数必须设置为“image”，`filename` 参数可以设置为任何字符串。 窗体内容是图像的二进制文件。 可以上传的最大图像大小为 1 MB。 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

本文包括了一个简单的控制台应用程序，它发送必应视觉搜索 API 请求并显示 JSON 搜索结果。 虽然此应用程序是用 Java 编写的，但 API 是一种 RESTful Web 服务，与任何可以发出 HTTP 请求并分析 JSON 的编程语言兼容。 


## <a name="prerequisites"></a>先决条件
对于本快速入门，你需要以 S9 价格层开始订阅，如[认知服务定价 - 必应搜索 API](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/) 中所示。 

若要在 Azure 门户中开始订阅，请执行以下操作：
1. 在 Azure 门户顶部显示 `Search resources, services, and docs` 的文本框中输入“BingSearchV7”。  
2. 在“市场”下的下拉列表中，选择 `Bing Search v7`。
3. 输入新资源的 `Name`。
4. 选择 `Pay-As-You-Go` 订阅。
5. 选择 `S9` 定价层。
6. 单击 `Enable` 即可开始订阅。

需要使用 [JDK 7 或 8](https://aka.ms/azure-jdks) 来编译和运行此代码。 可使用喜欢的 Java IDE（如果有）操作，但文本编辑器足以满足要求。

## <a name="running-the-application"></a>运行应用程序

下面展示了如何在 Java 中使用 MultipartEntityBuilder 上传图像。

若要运行此应用程序，请执行以下步骤：

1. 下载或安装 [gson 库](https://github.com/google/gson)。 也可以通过 Maven 获取它。
2. 在你喜欢使用的 IDE 或编辑器中新建一个 Java 项目。
3. 将提供的代码添加到一个名为 `VisualSearch.java` 的文件中。
4. 将 `subscriptionKey` 值替换为你的订阅密钥。
4. 将 `imagePath` 值替换为要上传的图像的路径。
5. 运行该程序。


```java
package uploadimage;

import java.util.*;
import java.io.*;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.2
 *
 * Once you have compiled or downloaded gson-2.8.2.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac BingImageSearch.java -classpath .;gson-2.8.2.jar -encoding UTF-8
 * java -cp .;gson-2.8.2.jar BingImageSearch
 */

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

// http://hc.apache.org/downloads.cgi (HttpComponents Downloads) HttpClient 4.5.5

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;


public class UploadImage2 {

    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere";
    static String imagePath = "<pathtoyourimagetouploadgoeshere>";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addBinaryBody("image", new File(imagePath))
                .build();

            HttpPost httpPost = new HttpPost(endpoint);
            httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            httpPost.setEntity(entity);
            HttpResponse response = httpClient.execute(httpPost);

            InputStream stream = response.getEntity().getContent();
            String json = new Scanner(stream).useDelimiter("\\A").next();

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(json));
        }
        catch (IOException e)
        {
            e.printStackTrace(System.out);
            System.exit(1);
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
    
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
    
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

