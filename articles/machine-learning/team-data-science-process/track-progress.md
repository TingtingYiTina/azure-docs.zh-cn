---
title: 数据科学项目的执行 - Azure 机器学习 | Microsoft Docs
description: 数据科学家如何跟踪数据科学项目的进度。
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/28/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 202ac89b8a281012dbcf5f4c4df11e97ba2c8c65
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441462"
---
# <a name="track-progress-of-data-science-projects"></a>跟踪数据科学项目的进度

数据科学组管理员、团队主管和项目主管需要跟踪项目的进度、已完成了哪些工作及是由谁完成的、待办事项列表上还剩下哪些工作。 

## <a name="azure-devops-dashboards"></a>Azure DevOps 仪表板
如果使用的是 Azure DevOps，则可以生成仪表板来跟踪与给定敏捷项目关联的活动和工作项。 

有关如何在 Azure DevOps 中创建和自定义仪表板与小组件的详细信息，请参阅以下几组说明：

- [添加和管理仪表板](https://docs.microsoft.com/azure/devops/report/dashboards/dashboards)
- [将小组件添加到仪表板](https://docs.microsoft.com/azure/devops/report/dashboards/add-widget-to-dashboard)。

## <a name="example-dashboard"></a>示例仪表板

下面是一个简单的示例仪表板，用于跟踪对关联存储库的敏捷数据科学项目，以及提交数的冲刺活动。 **左上角**面板显示：

- 当前冲刺的倒计时， 
- 在过去 7 天内的每个存储库提交数
- 特定用户的工作项。 

剩余面板显示项目的累积流图 (CFD)、燃尽和燃起图：

- **左下角**：CFD 中某一给定状态的工作的数量显示为灰色，蓝色提交并在完成为绿色批准。
- **右上角**：燃尽图有待与剩余时间完成的工作。
- **右下角**：燃起图的工作已完成工作的总量。

![仪表板](./media/track-progress/dashboard.png)

有关如何生成这些图表的说明，请参阅[仪表板](https://docs.microsoft.com/azure/devops/report/dashboards/)中的快速入门和教程。
 
## <a name="next-steps"></a>后续步骤

我们还提供了相应的演练，用于演示**具体方案**的操作过程的所有步骤。 [示例演练](walkthroughs.md)一文列出了相关步骤并以缩略图说明的形式提供了链接。 这些演练演示如何将云、本地工具和服务合并到工作流或管道中，以创建智能应用程序。 
