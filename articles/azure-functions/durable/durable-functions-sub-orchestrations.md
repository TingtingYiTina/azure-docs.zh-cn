---
title: Durable Functions 的子业务流程 - Azure
description: 如何从 Azure Functions 的 Durable Functions 扩展中的业务流程中调用业务流程。
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 32f8872737fdf6dd766ae8df8ef3ed47692e2c9c
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52636962"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Durable Functions 中的子业务流程 (Azure Functions)

除了调用活动函数之外，业务流程协调程序函数还可以调用其他业务流程协调程序函数。 例如，可以基于业务流程协调程序函数库构建更大的业务流程。 或者，你可以并行运行某个业务流程协调程序函数的多个实例。

一个业务流程协调程序函数可以通过调用 [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) 或 [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) 方法调用另一个业务流程协调程序函数。 [错误处理和修正](durable-functions-error-handling.md#automatic-retry-on-failure)一文提供了有关自动重试的更多信息。

从调用方的角度来看，子业务流程协调程序函数的行为与活动函数相同。 它们可以返回值，引发异常，并且父业务流程协调程序函数可以等待它们。

> [!NOTE]
> `CallSubOrchestratorAsync` 和 `CallSubOrchestratorWithRetryAsync` 方法尚不可在 JavaScript 中使用。

## <a name="example"></a>示例

以下示例说明了一个需要预配多台设备的 IoT（物联网）方案。 对于每台设备，都需要执行一个特定的业务流程，这可能类似于以下内容：

#### <a name="c"></a>C#

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string deviceId = ctx.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await ctx.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await ctx.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await ctx.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript（仅限 Functions v2）

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

此业务流程协调程序函数可以按现样用于一次性设备预配，也可以用作大型业务流程的一部分。 在后一种情况下，父业务流程协调程序函数可以使用 `CallSubOrchestratorAsync` (C#) 或 `callSubOrchestrator` (JS) API 调度 `DeviceProvisioningOrchestration` 的实例。

下面是一个示例，它展示了如何并行运行多个业务流程协调程序函数。

#### <a name="c"></a>C#

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string[] deviceIds = await ctx.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = ctx.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript（仅限 Functions v2）

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    for (const deviceId of deviceIds) {
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.push(provisionTask);
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解什么是任务中心以及如何配置它们](durable-functions-task-hubs.md)
