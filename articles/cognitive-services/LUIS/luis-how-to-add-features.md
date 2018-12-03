---
title: 用于增强实体检测的短语列表
titleSuffix: Azure Cognitive Services
description: 使用语言理解 (LUIS) 添加应用功能，可以改进对类别和模式的意向和实体的检测或预测
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: feb51cd55801addaf5ce2486e5527542f794bbc5
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52580944"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>使用短语列表来增强字词列表的信号

可以将功能添加到 LUIS 应用以提高其准确性。 特征通过提供某些字词和短语是应用域词汇表的一部分的提示来帮助 LUIS。 

[短语列表](luis-concept-feature.md)包括一组值（词或短语），它们属于同一个类，并且必须以同样的方式处理它们（例如城市或产品名称）。 LUIS 对其中一个值的了解也自动应用到其他值。 此列表不是匹配字词的封闭列表实体（完全文本匹配）。

短语列表添加到应用域的词汇中，作为这些字词的第二个 LUIS 信号。

## <a name="add-phrase-list"></a>添加短语列表

1. 单击“我的应用”页上的名称打开应用，单击“构建”，然后单击应用左侧面板中的“短语列表”。 

2. 在“短语列表”页上，单击“新建短语列表”。 
 
3. 在“添加短语列表”对话框中，键入“城市”作为短语列表的名称。 在“值”框中，键入短语列表的值。 可以一次键入一个值或者用逗号分隔的一组值，然后按 Enter。

    ![添加短语列表“城市”](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS 可以建议相关值，用于添加到短语列表。 单击“建议”，获取一组与添加的值在语义上相关的建议值。 可以单击任何建议的值，或单击“全部添加”添加所有值。

    ![短语列表建议的值](./media/luis-add-features/related-values.png)

5. 如果添加的短语列表值是可交换使用的替代值，则单击“这些值可以交换”。

    ![短语列表建议的值](./media/luis-add-features/interchangeable.png)

6. 单击“ **保存**”。 将“城市”短语列表添加到“短语列表”页。

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> 可以在“短语列表”页上，删除或取消激活上下文工具栏中的短语列表。

## <a name="next-steps"></a>后续步骤

添加、编辑、删除或停用短语列表后，再次[定型和测试应用](luis-interactive-test.md)，查看性能是否有所改善。
