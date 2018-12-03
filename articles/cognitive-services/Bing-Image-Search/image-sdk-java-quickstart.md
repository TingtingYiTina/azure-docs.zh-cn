---
title: 快速入门：通过适用于 Java 的必应图像搜索 SDK 搜索图像
description: 在本快速入门中，你将使用必应图像搜索 SDK（它是 API 的包装程序并包含相同的功能）创建你的第一个图像搜索。 这个简单的 Java 应用程序会发送图像搜索查询、分析 JSON 响应，并显示所返回的第一个图像的 URL。
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 81bd7356579b3e4f7b82497bb2163c85374fd0d9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316321"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-java"></a>快速入门，使用必应图像搜索 SDK 和 Java 搜索图像

在本快速入门中，你将使用必应图像搜索 SDK（它是 API 的包装程序并包含相同的功能）创建你的第一个图像搜索。 这个简单的 Java 应用程序会发送图像搜索查询、分析 JSON 响应，并显示所返回的第一个图像的 URL。

[GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingImageSearch/Quickstart) 上提供了此示例的源代码以及附加的错误处理和注释。

## <a name="prerequisites"></a>先决条件
在“搜索”下获取[认知服务访问密钥](https://azure.microsoft.com/try/cognitive-services/)。  另请参阅[认知服务定价 - 必应搜索 API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)。

最新版的 [Java 开发工具包](https://aka.ms/azure-jdks) (JDK)

通过使用 Maven、Gradle 或其他依赖项管理系统安装必应图像搜索 SDK 依赖项。 Maven POM 文件需要以下声明：

```xml
 <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-imagesearch</artifactId>
      <version>1.0.1</version>
    </dependency>
 </dependencies>
```

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>创建并初始化应用程序

1. 在喜欢的 IDE 或编辑器中新建一个 Java 项目，再向类实现中添加以下导入项：

    ```java
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchAPI;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.BingImageSearchManager;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImageObject;
    import com.microsoft.azure.cognitiveservices.search.imagesearch.models.ImagesModel;
    ```

2. 在 main 方法中，为订阅密钥创建变量，并创建搜索词。 然后，对必应图像搜索客户端进行实例化。

    ```java
    final String subscriptionKey = "COPY_YOUR_KEY_HERE";
    String searchTerm = "canadian rockies";
    //Image search client
    BingImageSearchAPI client = BingImageSearchManager.authenticate(subscriptionKey);
    ```

## <a name="send-a-search-request-to-the-bing-image-search-api"></a>向必应图像搜索 API 发送搜索请求

1. 使用 `bingImages().search()` 发送包含搜索查询的 HTTP 请求。 将响应保存为 `ImagesModel`。
   ```java
    ImagesModel imageResults = client.bingImages().search()
                .withQuery(searchTerm)
                .withMarket("en-us")
                .execute();
    ```

## <a name="parse-and-view-the-result"></a>分析并查看结果

分析响应中所返回的图像结果。
如果响应中包含搜索结果，则存储第一个结果并输出其详细信息，例如缩略图 URL、原始 URL 以及返回的图像的总数。  

```java
if (imageResults != null && imageResults.value().size() > 0) {
    // Image results
    ImageObject firstImageResult = imageResults.value().get(0);

    System.out.println(String.format("Total number of images found: %d", imageResults.value().size()));
    System.out.println(String.format("First image thumbnail url: %s", firstImageResult.thumbnailUrl()));
    System.out.println(String.format("First image content url: %s", firstImageResult.contentUrl()));
}
else {
        System.out.println("Couldn't find image results!");
     }

```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [必应图像搜索单页应用教程](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>另请参阅

* [什么是必应图像搜索？](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [尝试在线互动演示](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [获取免费的认知服务访问密钥](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)
* [Azure 认知服务 SDK 的 Java 示例](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)
* [Azure 认知服务文档](https://docs.microsoft.com/azure/cognitive-services)
* [必应图像搜索 API 参考](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
