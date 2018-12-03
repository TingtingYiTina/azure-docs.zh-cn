---
title: Durable Functions 中的永久业务流程 - Azure
description: 了解如何使用 Azure Functions 的 Durable Functions 扩展实现永久业务流程。
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 0e3a3476c3fca6329634c87f933f895ec582f364
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52636972"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Durable Functions 中的永久业务流程 (Azure Functions)

*永久业务流程*是永远不会结束的业务流程协调程序函数。 当希望将 [Durable Functions](durable-functions-overview.md) 用于聚合器和需要无限循环的任何方案时，永久业务流程非常有用。

## <a name="orchestration-history"></a>业务流程历史记录

如[检查点和重播](durable-functions-checkpointing-and-replay.md)中所述，Durable Task Framework 会跟踪每个函数业务流程的历史记录。 只要业务流程协调程序函数继续计划新工作，此历史记录就会不断增长。 如果业务流程协调程序函数进入无限循环并持续计划工作，则此历史记录可能会变得非常大并导致明显的性能问题。 *永久业务流程*概念是为了减轻需要无限循环的应用程序的此类问题而设计的。

## <a name="resetting-and-restarting"></a>重置和重启

业务流程协调程序函数通过调用 [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) 方法重置其状态，而不使用无限循环。 此方法采用单个 JSON 可序列化参数，该参数将成为用于生成下一个业务流程协调程序函数的新输入。

当调用 `ContinueAsNew` 时，实例会在其退出之前将一条消息排入其自己的队列。 该消息将使用新的输入值重启实例。 将保持同一实例 ID，但业务流程协调程序函数的历史记录实际上会被截断。

> [!NOTE]
> Durable Task Framework 会维护同一个实例 ID，但在内部会为由 `ContinueAsNew` 重置的业务流程协调程序函数创建一个新的“执行 ID”。 此执行 ID 通常不对外公开，但在调试业务流程执行时知道该 ID 可能比较有用。

## <a name="periodic-work-example"></a>定期工作示例

永久业务流程的一个用例是需要无限期执行定期工作的代码。

#### <a name="c"></a>C#

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript（仅限 Functions v2）

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("DoCleanup");

    // sleep for one hour between cleanups
    const nextCleanup = moment.utc(context.df.currentUtcDateTime).add(1, "h");
    yield context.df.createTimer(nextCleanup);

    context.df.continueAsNew(undefined);
});
```

此示例与计时器触发的函数之间的区别是此处的清理触发时间不基于计划。 例如，每小时执行某个函数的 CRON 计划将在 1:00、2:00 和 3:00 等时间执行，并且可能会遇到重叠问题。 不过，在此示例中，如果清理花费 30 分钟，则它将计划在 1:00、2:30、4:00 等时间执行，因此不可能重叠。

## <a name="exit-from-an-eternal-orchestration"></a>从永久业务流程退出

如果业务流程协调程序函数需要最终完成，则你需要做的全部工作“不是”调用 `ContinueAsNew` 而是让函数退出。

如果业务流程协调程序函数处于无限循环中并且需要停止，则使用 [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) 方法将其停止。 有关详细信息，请参阅[实例管理](durable-functions-instance-management.md)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何实现单一实例业务流程](durable-functions-singletons.md)
