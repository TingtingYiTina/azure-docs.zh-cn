---
title: 在 Durable Functions 中处理外部事件 - Azure
description: 了解如何在 Azure Functions 的 Durable Functions 扩展中处理外部事件。
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 98380cc5b9daff314283ac4e45e5edf7b5601e1b
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52636992"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>在 Durable Functions 中处理外部事件 (Azure Functions)

业务流程协调程序函数能够等待和侦听外部事件。 [Durable Functions](durable-functions-overview.md) 的此功能对于处理人机交互或其他外部触发器通常比较有用。

## <a name="wait-for-events"></a>等待事件

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) 方法允许业务流程协调程序函数以异步方式等待和侦听外部事件。 侦听业务流程协调程序函数声明了事件的“名称”和它期望收到的“数据形态”。

#### <a name="c"></a>C#

```csharp
[FunctionName("BudgetApproval")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool approved = await context.WaitForExternalEvent<bool>("Approval");
    if (approved)
    {
        // approval granted - do the approved action
    }
    else
    {
        // approval denied - send a notification
    }
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript（仅限 Functions v2）

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const approved = yield context.df.waitForExternalEvent("Approval");
    if (approved) {
        // approval granted - do the approved action
    } else {
        // approval denied - send a notification
    }
});
```

前面的示例侦听特定单个事件，并在收到该事件时执行操作。

可以同时侦听多个事件，像以下示例中一样，以下示例等待三个可能的事件通知之一。

#### <a name="c"></a>C#

```csharp
[FunctionName("Select")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var event1 = context.WaitForExternalEvent<float>("Event1");
    var event2 = context.WaitForExternalEvent<bool>("Event2");
    var event3 = context.WaitForExternalEvent<int>("Event3");

    var winner = await Task.WhenAny(event1, event2, event3);
    if (winner == event1)
    {
        // ...
    }
    else if (winner == event2)
    {
        // ...
    }
    else if (winner == event3)
    {
        // ...
    }
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript（仅限 Functions v2）

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const event1 = context.df.waitForExternalEvent("Event1");
    const event2 = context.df.waitForExternalEvent("Event2");
    const event3 = context.df.waitForExternalEvent("Event3");

    const winner = yield context.df.Task.any([event1, event2, event3]);
    if (winner === event1) {
        // ...
    } else if (winner === event2) {
        // ...
    } else if (winner === event3) {
        // ...
    }
});
```

前面的示例侦听多个事件中的“任何一个”。 还可以等待“所有”事件。

#### <a name="c"></a>C#

```csharp
[FunctionName("NewBuildingPermit")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string applicationId = context.GetInput<string>();

    var gate1 = context.WaitForExternalEvent("CityPlanningApproval");
    var gate2 = context.WaitForExternalEvent("FireDeptApproval");
    var gate3 = context.WaitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    await Task.WhenAll(gate1, gate2, gate3);

    await context.CallActivityAsync("IssueBuildingPermit", applicationId);
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript（仅限 Functions v2）

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const applicationId = context.df.getInput();

    const gate1 = context.df.waitForExternalEvent("CityPlanningApproval");
    const gate2 = context.df.waitForExternalEvent("FireDeptApproval");
    const gate3 = context.df.waitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    yield context.df.Task.all([gate1, gate2, gate3]);

    yield context.df.callActivity("IssueBuildingPermit", applicationId);
});
```

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) 无限期地等待一些输入。  在等待时，可以安全地卸载函数应用。 对于此业务流程实例，如果某个事件到达，则会自动唤醒并立即处理该事件。

> [!NOTE]
> 如果函数应用使用消耗计划，当业务流程协调程序函数等待来自 `WaitForExternalEvent` 的任务时，无论它等待多久，都不会产生账单费用。

在 .NET 中，如果事件负载无法转换为预期类型 `T`，将会引发异常。

## <a name="send-events"></a>发送事件

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) 类的 [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) 方法发送 `WaitForExternalEvent` 等待的事件。  `RaiseEventAsync` 方法采用 *eventName* 和 *eventData* 作为参数。 事件数据必须是 JSON 可序列化的。

下面是一个示例队列触发的函数，它将“Approval”事件发送到一个业务流程协调程序函数实例。 业务流程实例 ID 来自队列消息的正文。

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript（仅限 Functions v2）
在 JavaScript 中，我们必须调用 rest api 来触发持久函数正在等待的事件。
以下代码使用“request”包。 以下方法可用于为任何持久函数实例引发任何事件

```js
function raiseEvent(instanceId, eventName) {
        var url = `<<BASE_URL>>/runtime/webhooks/durabletask/instances/${instanceId}/raiseEvent/${eventName}?taskHub=DurableFunctionsHub`;
        var body = <<BODY>>
            
        return new Promise((resolve, reject) => {
            request({
                url,
                json: body,
                method: "POST"
            }, (e, response) => {
                if (e) {
                    return reject(e);
                }

                resolve();
            })
        });
    }
```

<<BASE_URL>> 将是函数应用的基本 URL。 如果在本地运行代码，那么它将类似于 http://localhost:7071，在 Azure 中为 https://<<functionappname>>.azurewebsites.net


在内部，`RaiseEventAsync` 将正在等待的业务流程协调程序函数选取的消息排入队列。

> [!WARNING]
> 如果没有具有指定“实例 ID”的业务流程实例，或者如果该实例未在等待指定的“事件名称”，则会丢弃事件消息。 有关此行为的详细信息，请参阅 [GitHub 问题](https://github.com/Azure/azure-functions-durable-extension/issues/29)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何设置永久业务流程](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [运行等待外部事件的示例](durable-functions-phone-verification.md)

> [!div class="nextstepaction"]
> [运行等待人机交互的示例](durable-functions-phone-verification.md)

