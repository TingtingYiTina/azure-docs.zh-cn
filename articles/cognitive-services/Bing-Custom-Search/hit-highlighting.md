---
title: 使用修饰标记来突出显示文本 - 必应自定义搜索
titlesuffix: Azure Cognitive Services
description: 演示如何启用搜索响应中的文本修饰。
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: maheshb
ms.openlocfilehash: 1ab99a8a024103f4a15154ee5b76e7dc21da4ae6
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "52162103"
---
# <a name="using-decoration-markers-to-highlight-text"></a>使用修饰标记来突出显示文本

必应支持命中词突出显示，此功能会在某些答案的显示字符串中标记查询词（或必应认为相关的其他词）。 例如，网页的 `name`、`displayUrl` 和 `snippet` 字段可能会标记查询词。

默认情况下，必应不在显示字符串中包括突出显示标记。 要包括这些标记，请在请求中包括 `textDecorations` 查询参数，并将其设置为 **true**。 必应使用 E000 和 E001 Unicode 字符来标记查询词，对该词的开头和结尾进行标记。 例如，如果查询词为 Sailing Dinghy 且这两个单词中的任一个都存在于字段中，则会在命中词突出显示字符中包括该单词，如以下示例所示：  
  
![命中词突出显示](./media/bing-hit-highlighting.PNG) 

在用户界面中显示该字符串之前，需将 Unicode 字符替换为适合显示格式的字符。 例如，若要将文本显示为 HTML，可以将 E000 替换为 <b\>，并将 E001 替换为 </b\>，从而突出显示查询词。 如果不希望应用格式，请从字符串中删除这些标记。 

必应提供使用 Unicode 字符或 HTML 标记作为标记这一选项。 若要指定要使用的标记，请包括 `textFormat` 查询参数。 要使用 Unicode 字符来标记内容，请将 `textFormat` 设置为 Raw（默认值）；要使用 HTML 标记来标记内容，请将 `textFormat` 设置为 HTML。 
  
如果 `textDecorations` 为 **true**，必应会在答案的显示字符串中包括以下标记。 如果没有 HTML 等效项，则 HTML 表单元格为空。

|Unicode|HTML|Description
|-|-|-
|U+E000|\<b>|表示查询词的开头（命中词突出显示）
|U+E001|\</b>|表示查询词的结尾
|U+E002|\<i>|表示斜体内容的开头 
|U+E003|\</i>|表示斜体内容的结尾
|U+E004|\<br/>|表示换行
|U+E005||表示电话号码的开头
|U+E006||表示电话号码的结尾
|U+E007||表示地址的开头
|U+E008||表示地址的结尾
|U+E009|\&nbsp;|表示不间断的空格
|U+E00C|\<strong>|表示粗体内容的开头
|U+E00D|\</strong>|表示粗体内容的结尾
|U+E00E||表示其背景颜色应比其周围背景颜色浅的内容的开头
|U+E00F||表示其背景颜色应比其周围背景颜色浅的内容的结尾
|U+E010||表示其背景颜色应比其周围背景颜色深的内容的开头
|U+E011||表示其背景颜色应比其周围背景颜色深的内容的结尾
|U+E012|\<del>|表示应删除的内容的开头
|U+E013|\</del>|表示应删除的内容的结尾
|U+E016|\<sub>|表示下标内容的开头
|U+E017|\</sub>|表示下标内容的结尾
|U+E018|\<sup>|表示上标内容的开头
|U+E019|\</sup>|表示上标内容的结尾

以下示例显示了一个 `Computation` 答案，其中包含 log(2) 查询词的下标标记。 仅当“textDecoration”为 **true** 时，`expression` 字段才包含这些标记。

![计算标记](./media/bing-markers-computation.PNG) 

如果该请求未请求修饰，则表达式为 log10(2)。 
  
