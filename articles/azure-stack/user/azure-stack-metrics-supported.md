---
title: Azure Stack 上的 Azure Monitor 支持的指标 | Microsoft Docs
description: 了解 Azure Stack 上的 Azure Monitor 支持的指标。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2018
ms.author: mabrigg
ms.openlocfilehash: f8ef54393f3de00ae231c45c117e3a16a8d1aad1
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2018
ms.locfileid: "52725601"
---
# <a name="supported-metrics-with-azure-monitor-on-azure-stack"></a>Azure Stack 上的 Azure Monitor 支持的指标

*适用于：Azure Stack 集成系统*

可以从 Azure Stack 上的 Azure Monitor 检索指标，与全球 Azure 相同。 可以在门户中创建你的度量值、 从 REST API 获取它们或使用 PowerShell 或 CLI 查询它们。

下表列出了 Azure Stack 上的 Azure Monitor 指标管道所提供的指标。 若要查询和访问这些指标，需要 API 配置文件的 **2018-01-01** api-version 版本。 有关 API 配置文件和 Azure Stack 的详细信息，请参阅[管理 Azure Stack 中的 API 版本配置文件](azure-stack-version-profiles.md)。

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

| 度量值 | 指标显示名称 | 单位 | 聚合类型 | 描述 | 维度 |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| CPU 百分比 | CPU 百分比 | 百分比 | 平均数 | 虚拟机当前正在使用的已分配计算单元的百分比 | 无维度 |

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

| 度量值 | 指标显示名称 | 单位 | 聚合类型 | 描述 | 维度 |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | 使用的容量 | 字节 | 平均数 | 帐户使用的容量 | 无维度 |
| 事务 | 事务 | 计数 | 总数 | 向存储服务或指定的 API 操作发出的请求数。 此数值包括成功和失败的请求数，以及引发错误的请求数。 针对不同类型的响应数使用 ResponseType 维度。 | ResponseType、GeoType、ApiName |
| 入口 | 入口 | 字节 | 总数 | 流入的数据量（以字节为单位）。 此数值包括从外部客户端到 Azure 存储流入的数据量，以及流入 Azure 中的数据量。 | GeoType、ApiName |
| 出口 | 出口 | 字节 | 总数 | 流出的数据量（以字节为单位）。 此数值包括从外部客户端到 Azure 存储流出的数据量，以及流出 Azure 中的数据量。 因此，此数值不反映计费的流出量。 | GeoType、ApiName |
| SuccessServerLatency | 成功服务器延迟 | 毫秒 | 平均数 | 由 Azure 存储用于处理成功请求的平均延迟（以毫秒为单位）。 此值不包括 AverageE2ELatency 中指定的网络延迟。 | GeoType、ApiName |
| SuccessE2ELatency | 成功 E2E 延迟 | 毫秒 | 平均数 | 向存储服务或指定的 API 操作发出的成功请求的平均端到端延迟（以毫秒为单位）。 此值包括在 Azure 存储中读取请求、发送响应和接收响应确认所需的处理时间。 | GeoType、ApiName |
| 可用性 | 可用性 | 百分比 | 平均数 | 存储服务或指定的 API 操作的可用性百分比。 可用性通过由 TotalBillableRequests 值除以适用的请求数（其中包括引发意外错误的请求）计算得出。 所有意外错误都会导致存储服务或指定的 API 操作的可用性下降。 | GeoType、ApiName |

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

| 度量值 | 指标显示名称 | 单位 | 聚合类型 | 描述 | 维度 |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| BlobCapacity | Blob 容量 | 字节 | 总数 | 存储帐户的 Blob 服务所使用的存储量(以字节为单位)。 | /BlobType |
| BlobCount | Blob 计数 | 计数 | 总数 | 存储帐户 Blob 服务中的 Blob 数。 | /BlobType |
| ContainerCount | Blob 容器计数 | 计数 | 平均数 | 存储帐户 Blob 服务中的容器数。 | 无维度 |
| 事务 | 事务 | 计数 | 总数 | 向存储服务或指定的 API 操作发出的请求数。 此数值包括成功和失败的请求数，以及引发错误的请求数。 针对不同类型的响应数使用 ResponseType 维度。 | ResponseType、GeoType、ApiName |
| 入口 | 入口 | 字节 | 总数 | 流入的数据量（以字节为单位）。 此数值包括从外部客户端到 Azure 存储流入的数据量，以及流入 Azure 中的数据量。 | GeoType、ApiName |
| 出口 | 出口 | 字节 | 总数 | 流出的数据量（以字节为单位）。 此数值包括从外部客户端到 Azure 存储流出的数据量，以及流出 Azure 中的数据量。 因此，此数值不反映计费的流出量。 | GeoType、ApiName |
| SuccessServerLatency | 成功服务器延迟 | 毫秒 | 平均数 | 由 Azure 存储用于处理成功请求的平均延迟（以毫秒为单位）。 此值不包括 AverageE2ELatency 中指定的网络延迟。 | GeoType、ApiName |
| SuccessE2ELatency | 成功 E2E 延迟 | 毫秒 | 平均数 | 向存储服务或指定的 API 操作发出的成功请求的平均端到端延迟（以毫秒为单位）。 此值包括在 Azure 存储中读取请求、发送响应和接收响应确认所需的处理时间。 | GeoType、ApiName |
| 可用性 | 可用性 | 百分比 | 平均数 | 存储服务或指定的 API 操作的可用性百分比。 可用性通过由 TotalBillableRequests 值除以适用的请求数（其中包括引发意外错误的请求）计算得出。 所有意外错误都会导致存储服务或指定的 API 操作的可用性下降。 | GeoType、ApiName |

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

| 度量值 | 指标显示名称 | 单位 | 聚合类型 | 描述 | 维度 |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| TableCapacity | 表容量 | 字节 | 平均数 | 存储帐户的表服务所使用的存储量(以字节为单位)。 | 无维度 |
| TableCount | 表计数 | 计数 | 平均数 | 存储帐户的表服务中的表数量。 | 无维度 |
| TableEntityCount | 表实体计数 | 计数 | 平均数 | 存储帐户的表服务中的表实体数。 | 无维度 |
| 事务 | 事务 | 计数 | 总数 | 向存储服务或指定的 API 操作发出的请求数。 此数值包括成功和失败的请求数，以及引发错误的请求数。 针对不同类型的响应数使用 ResponseType 维度。 | ResponseType、GeoType、ApiName |
| 入口 | 入口 | 字节 | 总数 | 流入的数据量（以字节为单位）。 此数值包括从外部客户端到 Azure 存储流入的数据量，以及流入 Azure 中的数据量。 | GeoType、ApiName |
| 出口 | 出口 | 字节 | 总数 | 流出的数据量（以字节为单位）。 此数值包括从外部客户端到 Azure 存储流出的数据量，以及流出 Azure 中的数据量。 因此，此数值不反映计费的流出量。 | GeoType、ApiName |
| SuccessServerLatency | 成功服务器延迟 | 毫秒 | 平均数 | 由 Azure 存储用于处理成功请求的平均延迟（以毫秒为单位）。 此值不包括 AverageE2ELatency 中指定的网络延迟。 | GeoType、ApiName |
| SuccessE2ELatency | 成功 E2E 延迟 | 毫秒 | 平均数 | 向存储服务或指定的 API 操作发出的成功请求的平均端到端延迟（以毫秒为单位）。 此值包括在 Azure 存储中读取请求、发送响应和接收响应确认所需的处理时间。 | GeoType、ApiName |
| 可用性 | 可用性 | 百分比 | 平均数 | 存储服务或指定的 API 操作的可用性百分比。 可用性通过由 TotalBillableRequests 值除以适用的请求数（其中包括引发意外错误的请求）计算得出。 所有意外错误都会导致存储服务或指定的 API 操作的可用性下降。 | GeoType、ApiName |

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

| 度量值 | 指标显示名称 | 单位 | 聚合类型 | 描述 | 维度 |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | 队列容量 | 字节 | 平均数 | 存储帐户的队列服务所使用的存储量(以字节为单位)。 | 无维度 |
| QueueCount | 队列计数 | 计数 | 平均数 | 存储帐户的队列服务中的队列数。 | 无维度 |
| QueueMessageCount | 队列消息计数 | 计数 | 平均数 | 存储帐户的队列服务中队列消息的大致数量。 | 无维度 |
| 事务 | 事务 | 计数 | 总数 | 向存储服务或指定的 API 操作发出的请求数。 此数值包括成功和失败的请求数，以及引发错误的请求数。 针对不同类型的响应数使用 ResponseType 维度。 | ResponseType、GeoType、ApiName |
| 入口 | 入口 | 字节 | 总数 | 流入的数据量（以字节为单位）。 此数值包括从外部客户端到 Azure 存储流入的数据量，以及流入 Azure 中的数据量。 | GeoType、ApiName |
| 出口 | 出口 | 字节 | 总数 | 流出的数据量（以字节为单位）。 此数值包括从外部客户端到 Azure 存储流出的数据量，以及流出 Azure 中的数据量。 因此，此数值不反映计费的流出量。 | GeoType、ApiName |
| SuccessServerLatency | 成功服务器延迟 | 毫秒 | 平均数 | 由 Azure 存储用于处理成功请求的平均延迟（以毫秒为单位）。 此值不包括 AverageE2ELatency 中指定的网络延迟。 | GeoType、ApiName |
| SuccessE2ELatency | 成功 E2E 延迟 | 毫秒 | 平均数 | 向存储服务或指定的 API 操作发出的成功请求的平均端到端延迟（以毫秒为单位）。 此值包括在 Azure 存储中读取请求、发送响应和接收响应确认所需的处理时间。 | GeoType、ApiName |
| 可用性 | 可用性 | 百分比 | 平均数 | 存储服务或指定的 API 操作的可用性百分比。 可用性通过由 TotalBillableRequests 值除以适用的请求数（其中包括引发意外错误的请求）计算得出。 所有意外错误都会导致存储服务或指定的 API 操作的可用性下降。 | GeoType、ApiName |

## <a name="next-steps"></a>后续步骤

详细了解 [Azure Stack 上的 Azure Monitor](azure-stack-metrics-azure-data.md)。
