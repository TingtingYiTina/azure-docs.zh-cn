---
title: 配置 Azure Log Analytics 中的数据源 | Microsoft Docs
description: 数据源定义 Log Analytics 从代理和其他连接的源所收集的数据。  本文介绍有关 Log Analytics 如何使用数据源的概念，详细解释如何配置数据源，并对不同的可用数据源进行概要介绍。
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 8dad4c11ac309d959675d525fbeb48fe385cf4a5
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336396"
---
# <a name="data-sources-in-log-analytics"></a>Log Analytics 中的数据源
Log Analytics 从已连接的数据源收集数据并将其存储在 Log Analytics 工作区中。  从每个源收集的数据由所配置的数据源定义。  Log Analytics 中的数据以一组记录的形式存储。  每个数据源将创建具有某种特殊类型的记录，而每个类型都具有自己的一组属性。

![Log Analytics - 数据收集](./media/agent-data-sources/overview.png)

虽然[管理解决方案](../../azure-monitor/insights/solutions.md)同样从连接的数据源收集数据并在 Log Analytics 中创建记录，但数据源还是有别于管理解决方案的。  除了收集数据外，解决方案通常还包括日志搜索和视图，以帮助你分析特定应用程序或服务的操作。


## <a name="summary-of-data-sources"></a>数据源概要介绍
下表列出了 Log Analytics 中当前可用的数据源。  每个数据源都链接到一篇单独的文章，提供该数据源的详细信息。   它还提供了这些解决方案将数据收集到 Log Analytics 中时采用的方法和频率的相关信息。  可以使用本文中的信息来了解可用的各种解决方案，并了解各种管理解决方案的数据流和连接要求。 有关列的说明，请参阅 [Azure 中的管理解决方案的数据收集详细信息](../../azure-monitor/insights/solutions-inventory.md)。


| 数据源 | 平台 | Microsoft Monitoring Agent | Operations Manager 代理 | Azure 存储 | 需要 Operations Manager？ | Operations Manager 代理数据通过管理组发送 | 收集频率 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [自定义日志](data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | 到达时 |
| [自定义日志](data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | 到达时 |
| [IIS 日志](data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |依赖于日志文件滚动更新设置 |
| [性能计数器](data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |根据计划，最小值为 10 秒 |
| [性能计数器](data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |根据计划，最小值为 10 秒 |
| [Syslog](data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |来自 Azure 存储：10 分钟；来自代理：到达时 |
| [Windows 事件日志](data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | 到达时 |


## <a name="configuring-data-sources"></a>配置数据源
可以从 Log Analytics **高级设置**中的“数据”菜单配置数据源。  任何配置都将传送到工作区中所有已连接的数据源。  当前不能从此配置中排除任何代理。

![配置 Windows 事件](./media/agent-data-sources/configure-events.png)

1. 在 Azure 门户中，选择“Log Analytics”> 你的工作区 >“高级设置”。
2. 选择“**数据**”。
3. 单击要配置的数据源。
4. 按照上表中每个数据源链接到的文档，了解有关其配置的详细信息。


## <a name="data-collection"></a>数据收集
数据源配置会在几分钟内传送到与 Log Analytics 直接连接的各个代理。  指定的数据从代理收集，并按特定于每个数据源的时间间隔直接传送到 Log Analytics。  请参阅每个数据源的文档以了解详情。

对于已连接管理组中的 System Center Operations Manager 代理，数据源配置默认以每 5 分钟的间隔转换成管理包并传送到管理组。  代理会下载任何其他的管理包，并收集指定的数据。 根据数据源的不同，数据或者被发送到管理服务器，再由管理服务器转发到 Log Analytics；或者不通过管理服务器，由代理将数据发送到 Log Analytics。 有关详细信息，请参阅 [Azure 中的管理解决方案的数据收集详细信息](../../azure-monitor/insights/solutions-inventory.md)。  可以在[配置与 System Center Operations Manager 的集成](../../log-analytics/log-analytics-om-agents.md)中阅读有关连接 Operations Manager 和 Log Analytics 以及修改配置传送频率的详细信息。

如果代理无法连接到 Log Analytics 或 Operations Manager，将继续收集在建立连接时传送的数据。  如果数据量达到客户端的最大缓存大小，或者如果代理无法在 24 小时内建立连接，则可能会丢失数据。

## <a name="log-analytics-records"></a>Log Analytics 记录
Log Analytics 所收集的所有数据都作为记录存储在工作区中。  按不同数据源收集的记录具有其自己的属性集，并由其“**类型**”属性来识别。  有关每种记录类型的详细信息，请参阅每个数据源和解决方案的相关文档。

## <a name="next-steps"></a>后续步骤
* 了解[解决方案](../../azure-monitor/insights/solutions.md)如何将功能添加到 Log Analytics，以及如何将数据收集到工作区中。
* 了解[日志搜索](../../log-analytics/log-analytics-queries.md)以便分析从数据源和解决方案中收集的数据。  
* 配置[警报](../../monitoring-and-diagnostics/monitoring-overview-alerts.md)以便主动向你通知从数据源和解决方案中收集的关键数据。
