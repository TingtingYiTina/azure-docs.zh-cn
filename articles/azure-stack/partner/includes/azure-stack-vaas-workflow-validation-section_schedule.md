---
author: mattbriggs
ms.service: azure-stack
ms.topic: include
ms.date: 11/26/2018
ms.author: mabrigg
ms.openlocfilehash: be9b3b3a9cbb39386d13f76c595ce5e34688adac
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336060"
---
验证工作流中**安排**测试使用工作流创建期间指定的工作流级别公用参数 (请参阅[工作流通用参数作为服务AzureStack验证](../azure-stack-vaas-parameters.md)). 如果任何测试参数值变得无效，您必须 resupply 它们中所述[修改工作流参数](../azure-stack-vaas-monitor-test.md#change-workflow-parameters)。

> [!NOTE]
> 计划高于现有实例的一个验证测试，将在门户中创建的新实例来代替旧实例。 旧实例的日志将保留，但不能从门户访问。  
测试成功，完成后**计划**操作将被禁用。

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](azure-stack-vaas-workflow-step_select-agent.md)]

1. 选择**计划**从上下文菜单打开的提示的计划的测试实例。

1. 查看测试参数，然后选择**提交**来计划执行测试。