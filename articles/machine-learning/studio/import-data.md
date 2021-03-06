---
title: 将数据导入机器学习工作室 | Microsoft Docs
description: 如何将数据从各种数据源导入 Azure 机器学习工作室。 了解受支持的数据类型和数据格式。
keywords: 导入数据, 数据格式, 数据类型, 数据源, 训练数据
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 575e17f214561bde06b70bf87c4895ae379659f2
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344716"
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>将训练数据从各种数据源导入 Azure 机器学习工作室

若要在机器学习工作室中使用自己的数据来开发和训练预测分析解决方案，可以使用来自以下源的数据： 

* [**本地文件**](import-data-from-local-file.md) - 提前从硬盘驱动器加载本地数据，以便在工作区中创建数据集模块
* [**联机数据源**](import-data-from-online-sources.md) - 运行试验时使用[导入数据][import-data]模块，访问来自多个联机源之一的数据
* [**机器学习工作室试验**](import-data-from-an-experiment.md) - 使用在机器学习工作室中保存为数据集的数据
* [**本地 SQL Server 数据库**](use-data-from-an-on-premises-sql-server.md) - 使用来自本地 SQL Server 数据库的数据，而无需手动复制数据

> [!NOTE]
> 机器学习工作室中提供有许多可用于训练数据的示例数据集。 若要了解这些示例数据集，请参阅[在 Azure 机器学习工作室中使用示例数据集](use-sample-datasets.md)。
> 
> 

此介绍性文章还讨论了如何准备数据以在机器学习工作室中使用，并介绍了受支持的数据格式和数据类型。

> [!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>准备数据以在机器学习工作室中使用
机器学习工作室旨在使用矩形或表格数据，例如从数据库分隔或结构化数据的文本数据，但在某些情况下，可能会使用非矩形数据。

最好使用相对干净的数据。 也就是说，在将数据上传到试验中之前，请注意字符串未加引号等问题。

但是，机器学习工作室中有些模块可在试验内启用某些数据操作。 根据要使用的机器学习算法，可能需要确定处理数据结构问题（如缺失值和稀疏数据）的方法，并且提供有模块帮助处理这些问题。 请查看模块控制板的“数据转换”部分，了解执行这些函数的模块。

通过单击输出端口，任何时候都可在试验中查看或下载模块生成的数据。 因模块不同，可能有不同的下载选项可用，或能够在机器学习工作室的 Web 浏览器中显示数据。

## <a name="data-formats-and-data-types-supported"></a>受支持的数据格式和数据类型
可将大量数据类型导入试验，具体取决于用于导入数据的机制以及数据的来源：

* 纯文本 (.txt)
* 逗号分隔值 (CSV)，带有标头 (.csv) 或不带标头 (.nh.csv)
* 制表符分隔值 (TSV)，带有标头 (.tsv) 或不带标头 (.nh.tsv)
* Excel 文件
* Azure 表
* Hive 表
* SQL 数据库表
* OData 值
* SVMLight 数据 (.svmlight)（有关格式的信息，请参阅 [SVMLight 定义](http://svmlight.joachims.org/)）
* 属性关系文件格式 (ARFF) 数据 (.arff)（有关格式的信息，请参阅 [ARFF 定义](http://weka.wikispaces.com/ARFF)）
* Zip 文件 (.zip)
* R 对象或工作区文件 (.RData)

如果导入包括元数据的数据（如 ARFF 格式），机器学习工作室将使用此元数据定义每个列的标题和数据类型。

如果导入不包括此元数据的数据（如 TSV 或 CSV 格式），机器学习工作室将通过数据采样推断每个列的数据类型。 如果数据也没有列标题，机器学习工作室将提供默认名称。

可使用[编辑元数据][edit-metadata]显式指定或更改列的标题和数据类型。

机器学习工作室可识别以下**数据类型**：

* String
* Integer
* Double
* Boolean
* DateTime
* TimeSpan

机器学习工作室使用名为“数据表”的内部数据类型在模块间传递数据。 可使用[转换为数据集][convert-to-dataset]模块，将数据显式转换为数据表格式。

任何接受格式而不是数据表的模块都会在不提示的情况将数据转换为数据表，然后再将其传递到下一个模块。

如有必要，可使用其他转换模块，将数据表格式转换回 CSV、TSV、ARFF 或 SVMLight 格式。
请查看模块控制板的“数据格式转换”部分，了解执行这些函数的模块。

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
