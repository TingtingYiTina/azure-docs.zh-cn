---
title: Azure SQL 数据库指标和诊断日志记录 | Microsoft Docs
description: 了解如何配置 Azure SQL 数据库以存储资源使用情况、连接性和查询执行统计信息。
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: b903d0ddbccac8fe4fa8b251d409bd8addebb435
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52425993"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL 数据库指标和诊断日志记录 

Azure SQL 数据库、弹性池、托管实例和托管实例中的数据库可发出指标和诊断日志，以便更轻松地进行性能监视。 可配置数据库，将资源使用情况、辅助角色和会话以及连接性流式传输到以下 Azure 资源之一：

* **Azure SQL Analytics**：用于具有报告、警报和缓解功能的集成式 Azure 数据库智能性能监视解决方案。
* **Azure 事件中心**：用于将 SQL 数据库遥测与自定义监视解决方案或热门管道集成。
* **Azure 存储**：用于低价存档大量遥测数据。

    ![体系结构](./media/sql-database-metrics-diag-logging/architecture.png)

若要了解各种 Azure 服务支持的指标和日志类别，建议阅读以下主题：

* [Microsoft Azure 中的指标概述](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Azure 诊断日志概述](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

## <a name="enable-logging-of-diagnostics-telemetry"></a>启用诊断遥测的日志记录

参考本文档的第一部分为数据库启用诊断遥测，参考第二部分为弹性池或托管实例启用诊断遥测。 参考本文档的后面几个部分将 Azure SQL Analytics 配置为监视工具用于查看流式传输的数据库诊断遥测数据。

> [!NOTE]
> 如果你正在使用弹性池或托管实例，同时为数据库启用了诊断遥测，则我们还建议为这些资源（弹性池或托管实例）启用诊断遥测。 这是因为，充当数据库容器角色的弹性池和托管实例自身的诊断遥测不同于单个数据库的诊断遥测。 
>

可使用以下方法之一来启用和管理指标与诊断遥测日志记录：

- Azure 门户
- PowerShell
- Azure CLI
- Azure Monitor REST API 
- Azure 资源管理器模板

启用指标和诊断日志记录时，需要指定收集所选数据的 Azure 资源目标。 可用选项包括：

- Azure SQL 分析
- Azure 事件中心
- Azure 存储

可预配新的 Azure 资源或选择现有资源。 在使用诊断设置选项选择资源之后，需指定要收集的数据。

## <a name="enable-logging-for-azure-sql-database-or-databases-in-managed-instance"></a>为 Azure SQL 数据库或托管实例中的数据库启用日志记录

默认情况下，不会对 SQL 数据库以及托管实例中的数据库启用指标与诊断日志记录。

以下诊断遥测适用于 Azure SQL 数据库以及托管实例中的数据库的集合：

| 数据库的监视遥测 | Azure SQL 数据库支持 | 托管实例中的数据库支持 |
| :------------------- | ------------------- | ------------------- |
| [所有指标](sql-database-metrics-diag-logging.md#all-metrics)：包含 DTU/CPU 百分比、DTU/CPU 限制、物理数据读取百分比、日志写入百分比、成功/失败/防火墙阻止的连接数、会话百分比、辅助角色百分比、存储、存储百分比和 XTP 存储百分比。 | 是 | 否 |
| [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics)：包含有关查询运行时统计信息（如 CPU 使用率、查询持续时间统计信息等）的信息。 | 是 | 是 |
| [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics)：包含有关查询等待统计信息的信息，可告知用户查询在什么项上等待，如 CPU、日志、锁定。 | 是 | 是 |
| [错误](sql-database-metrics-diag-logging.md#errors-dataset)：包含有关此数据库发生的 SQL 错误的信息。 | 是 | 否 |
| [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset)：包含有关数据库针对不同等待类型花费多少时间等待的信息。 | 是 | 否 |
| [超时](sql-database-metrics-diag-logging.md#time-outs-dataset)：包含有关在数据库上发生的超时的信息。 | 是 | 否 |
| [阻塞](sql-database-metrics-diag-logging.md#blockings-dataset)：包含有关在数据库上发生的阻塞事件的信息。 | 是 | 否 |
| [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset)：包含性能方面的智能见解。 [详细了解 Intelligent Insights](sql-database-intelligent-insights.md)。 | 是 | 是 |

### <a name="azure-portal"></a>Azure 门户

在 Azure 门户中通过每个数据库的诊断设置菜单，将 Azure SQL 数据库、托管实例中的数据库的诊断遥测数据配置为流式传输到 Azure 存储目标、事件中心或 Log Analytics。

### <a name="configure-streaming-of-diagnostics-telemetry-for-azure-sql-database"></a>为 Azure SQL 数据库配置诊断遥测流

   ![SQL 数据库图标](./media/sql-database-metrics-diag-logging/icon-sql-database-text.png)

若要为 **Azure SQL 数据库**启用诊断遥测流，请执行以下步骤：

1. 转到 Azure SQL 数据库资源
2. 选择“诊断设置”
3. 选择“启用诊断”（如果不存在以前的设置），或选择“编辑设置”来编辑以前的设置
- 最多可以创建三 (3) 个并行连接用于流式传输诊断遥测数据。 若要配置为使用多个并行连接将诊断数据流式传输到多个资源，请选择“+添加诊断设置”以创建附加的设置。

   ![为 SQL 数据库启用诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)

4. 键入设置名称 - 供自己参考
5. 选择要将数据库中的诊断数据流式传输到哪个资源：“存档到存储帐户”、“流式传输到事件中心”或“发送到 Log Analytics”
6. 对于标准监视体验，请选中数据库诊断日志遥测对应的复选框：“SQLInsights”、“AutomaticTuning”、“QueryStoreRuntimeStatistics”、“QueryStoreWaitStatistics”、“Errors”、“DatabaseWaitStatistics”、“Timeouts”、“Blocks”、“Deadlocks”。 此遥测基于事件，提供标准监视体验。
7. 对于高级监视体验，请选中“AllMetrics”对应的复选框。 如前所述，这是 1 分钟的数据库诊断遥测。 
8. 单击“保存”

   ![为 SQL 数据库配置诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)

> [!NOTE]
> 无法从数据库诊断设置启用安全审核日志。 若要启用审核日志流式传输，请参阅[为数据库设置审核](sql-database-auditing.md#subheading-2)，另请参阅 [Azure Log Analytics 和 Azure 事件中心内的 SQL 审核日志](https://blogs.msdn.microsoft.com/sqlsecurity/2018/09/13/sql-audit-logs-in-azure-log-analytics-and-azure-event-hubs/)。
>

> [!TIP]
> 针对要监视的每个 Azure SQL 数据库重复上述步骤。 
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-databases-in-managed-instance"></a>为托管实例中的数据库配置诊断遥测流

   ![托管实例中的数据库图标](./media/sql-database-metrics-diag-logging/icon-mi-database-text.png)

若要为**托管实例中的数据库**启用诊断遥测流，请执行以下步骤：

1. 转到托管实例中的数据库
2. 选择“诊断设置”
3. 选择“启用诊断”（如果不存在以前的设置），或选择“编辑设置”来编辑以前的设置
- 最多可以创建三 (3) 个并行连接用于流式传输诊断遥测数据。 若要配置为使用多个并行连接将诊断数据流式传输到多个资源，请选择“+添加诊断设置”以创建附加的设置。

   ![为托管实例数据库启用诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

4. 键入设置名称 - 供自己参考
5. 选择要将数据库中的诊断数据流式传输到哪个资源：“存档到存储帐户”、“流式传输到事件中心”或“发送到 Log Analytics”
6. 选中数据库诊断遥测对应的复选框：“SQLInsights”、“QueryStoreRuntimeStatistics”、“QueryStoreWaitStatistics”和“Errors”
7. 单击“保存”

   ![为托管实例数据库配置诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)

> [!TIP]
> 针对要监视的每个托管实例中数据库重复上述步骤。
>

## <a name="enable-logging-for-elastic-pools-or-managed-instance"></a>为弹性池或托管实例启用日志记录

用作数据库容器的弹性池和托管实例自身的诊断遥测不同于数据库。 默认不会启用此诊断遥测。 

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>配置弹性池的诊断遥测流

   ![弹性池图标](./media/sql-database-metrics-diag-logging/icon-elastic-pool-text.png)

以下诊断遥测可用于弹性池资源集合：

| 资源 | 监视遥测数据 |
| :------------------- | ------------------- |
| **弹性池** | [所有指标](sql-database-metrics-diag-logging.md#all-metrics)包含 eDTU/CPU 百分比、eDTU/CPU 限制、物理数据读取百分比、日志写入百分比、会话百分比、辅助角色百分比、存储、存储百分比、存储限制，以及 XTP 存储百分比。 |

若要为**弹性池资源**启用诊断遥测流，请执行以下步骤：

1. 在 Azure 门户中转到弹性池资源
2. 选择“诊断设置”
3. 选择“启用诊断”（如果不存在以前的设置），或选择“编辑设置”来编辑以前的设置

   ![为弹性池启用诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

4. 键入设置名称 - 供自己参考
5. 选择要将弹性池中的诊断数据流式传输到哪个资源：“存档到存储帐户”、“流式传输到事件中心”或“发送到 Log Analytics”
6. 如果选择了 Log Analytics，请选择“配置”，并通过选择“+ 创建新工作区”来创建新工作区；或者选择现有的工作区
7. 选中弹性池诊断遥测 **AllMetrics** 对应的复选框
8. 单击“保存”

   ![为弹性池配置诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)

> [!TIP]
> 针对要监视的每个弹性池重复上述步骤。
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instance"></a>为托管实例配置诊断遥测流

   ![托管实例图标](./media/sql-database-metrics-diag-logging/icon-managed-instance-text.png)

以下诊断遥测适用于托管实例资源的集合：

| 资源 | 监视遥测数据 |
| :------------------- | ------------------- |
| **托管实例** | [ResourceUsageStats](sql-database-metrics-diag-logging.md#resource-usage-stats) 包含 vCore 计数、平均 CPU 百分比、IO 请求数、读取/写入的字节数、保留的存储空间、已使用的存储空间。 |

若要为**托管实例资源**启用诊断遥测流，请执行以下步骤：

1. 在 Azure 门户中转到托管实例资源
2. 选择“诊断设置”
3. 选择“启用诊断”（如果不存在以前的设置），或选择“编辑设置”来编辑以前的设置

   ![为托管实例启用诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-enable.png)

4. 键入设置名称 - 供自己参考
5. 选择要将弹性池中的诊断数据流式传输到哪个资源：“存档到存储帐户”、“流式传输到事件中心”或“发送到 Log Analytics”
6. 如果选择了 Log Analytics，请创建或使用现有的工作区
7. 选中诊断遥测 **ResourceUsageStats** 对应的复选框
8. 单击“保存”

   ![为托管实例配置诊断](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-selection.png)

> [!TIP]
> 针对要监视的每个托管实例重复上述步骤。
>

### <a name="powershell"></a>PowerShell

若要使用 PowerShell 启用指标和诊断日志记录，请使用以下命令：

- 若要允许在存储帐户中存储诊断日志，请使用以下命令：

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   存储帐户 ID 是需要向其发送日志的存储帐户的资源 ID。

- 要允许将诊断日志流式传输到事件中心，请使用以下命令：

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure 服务总线规则 ID 是以下格式的字符串：

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- 若要允许将诊断日志发送到 Log Analytics 工作区，请使用以下命令：

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- 可以使用以下命令获取 Log Analytics 工作区的资源 ID：

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

可以组合这些参数以启用多个输出选项。

### <a name="to-configure-multiple-azure-resources"></a>配置多个 Azure 资源

若要支持多个订阅，请使用 [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/)（通过 PowerShell 启用 Azure 资源指标日志记录）中的 PowerShell 脚本。

在执行脚本时提供工作区资源 ID &lt;$WSID&gt; 作为参数 (Enable-AzureRMDiagnostics.ps1)，以便将诊断数据从多个资源发送到工作区。 若要获取要向其发送诊断数据的工作区 ID &lt;$WSID&gt;，请将 &lt;subID&gt; 替换为订阅 ID，将 replace &lt;RG_NAME&gt; 替换为资源组名称，并将 &lt;WS_NAME&gt; 替换为以下脚本中的工作区名称。

- 若要配置多个 Azure 资源，请使用以下命令：

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

### <a name="azure-cli"></a>Azure CLI

若要使用 Azure CLI 启用指标和诊断日志记录，请使用以下命令：

- 若要允许在存储帐户中存储诊断日志，请使用以下命令：

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   存储帐户 ID 是需要向其发送日志的存储帐户的资源 ID。

- 要允许将诊断日志流式传输到事件中心，请使用以下命令：

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   服务总线规则 ID 是以下格式的字符串：

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- 若要允许将诊断日志发送到 Log Analytics 工作区，请使用以下命令：

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

可以组合这些参数以启用多个输出选项。

### <a name="rest-api"></a>REST API

阅读有关如何[使用 Azure Monitor REST API 更改诊断设置](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)的信息。 

### <a name="resource-manager-template"></a>资源管理器模板

阅读有关如何[在创建资源时使用资源管理器模板启用诊断设置](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md)的信息。 

## <a name="stream-into-azure-sql-analytics"></a>流式传输到 Azure SQL Analytics 

Azure SQL Analytics 是一种云监视解决方案，用于通过单个视图跨多个订阅大规模监视 Azure SQL 数据库、弹性池和托管实例的性能。 它通过内置智能来收集和直观显示重要的 Azure SQL 数据库性能指标，以便进行性能故障排除。

![Azure SQL Analytics 概述](../log-analytics/media/log-analytics-azure-sql/azure-sql-sol-overview.png)

在门户中使用诊断设置边栏选项卡上的内置“发送到 Log Analytics”选项，可将 SQL 数据库指标和诊断日志流式处理到 Azure SQL Analytics。 此外，还可以通过 PowerShell cmdlet、Azure CLI 或 Azure Monitor REST API 使用诊断设置来启用 Log Analytics。

### <a name="installation-overview"></a>安装概述

使用 Azure SQL Analytics 可以方便地监视 SQL 数据库群。 需要三个步骤：

1. 从 Azure 市场创建 Azure SQL Analytics 解决方案
2. 在解决方案中创建监视工作区
3. 将数据库配置为向创建的工作区流式传输诊断遥测数据。

如果使用的是弹性池或托管实例，则除了配置数据库诊断遥测以外，还要配置从这些资源流式传输诊断遥测数据。

### <a name="create-azure-sql-analytics-resource"></a>创建 Azure SQL Analytics 资源

1. 在 Azure 市场中搜索 Azure SQL Analytics 并选择它

   ![在门户中搜索 Azure SQL Analytics](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)
   
2. 在解决方案的概述屏幕上选择“创建”

3. 在“Azure SQL Analytics”窗体中填写所需的附加信息：工作区名称、订阅、资源组、位置和定价层。
 
   ![在门户中配置 Azure SQL Analytics](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. 选择“确定”以确认，并选择“创建”以完成操作

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>将数据库配置为记录指标和诊断日志

通过 Azure 门户配置数据库记录其指标的位置是最简单的方式 - 请参阅上文。 在门户中，转到 SQL 数据库资源，然后选择“诊断设置”。

如果使用的是弹性池或托管实例，则还需要在这些资源中配置诊断设置，并将这些资源自身的诊断遥测数据流式传输到创建的工作区。

### <a name="use-the-sql-analytics-solution"></a>使用 SQL Analytics 解决方案

SQL Analytics 是一个分层仪表板，用户使用它可在 SQL 数据库资源的层次结构中移动。 若要了解如何使用 SQL Analytics 解决方案，请参阅[使用 SQL Analytics 解决方案监视 SQL 数据库](../log-analytics/log-analytics-azure-sql.md)。

## <a name="stream-into-event-hubs"></a>流式传输到事件中心

在门户中使用内置的“流式传输到事件中心”选项，可将 SQL 数据库指标和诊断日志流式传输到事件中心。 此外，还可以通过 PowerShell cmdlet、Azure CLI 或 Azure Monitor REST API 使用诊断设置来启用服务总线规则 ID。 

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>如何处理事件中心内的指标和诊断日志
将选定的数据流式传输到事件中心后，就离启动高级监视方案更进一步了。 事件中心充当事件管道的前门。 将数据收集到事件中心后，可以使用任何实时分析提供程序或批处理/存储适配器转换和存储这些数据。 事件中心将事件流的生成从这些事件的使用中分离出来。 通过这种方式，事件使用者可以访问自己的计划中的事件。 有关事件中心的详细信息，请参阅：

- [什么是 Azure 事件中心？](../event-hubs/event-hubs-what-is-event-hubs.md)
- [事件中心入门](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

可以通过以下几种方式使用流式传输功能：

* **通过将热路径数据流式传输到 Power BI 来查看服务运行状况**。 使用事件中心、流分析和 PowerBI，可以在 Azure 服务中轻松地将指标和诊断数据转换成几近实时的分析结果。 有关如何设置事件中心、如何使用流分析处理数据，以及如何使用 PowerBI 作为输出的概述，请参阅[流分析和 Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)。

* **将日志流式传输到第三方日志记录和遥测流**。 使用事件中心流式传输，可将指标和诊断日志引入不同的第三方监视和日志分析解决方案。 

* **生成自定义遥测和日志记录平台**。 如果已经有一个自定义生成的遥测平台，或者正想生成一个，可利用事件中心高度可缩放的发布-订阅功能，灵活地引入诊断日志。 请参阅 [Dan Rosanova 的指南：了解如何在全局规模的遥测平台中使用事件中心](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)。

## <a name="stream-into-storage"></a>流式传输到存储

在门户中使用内置的“存档到存储帐户”选项，可以在存储中存储 SQL 数据库指标和诊断日志。 此外，还可以通过 PowerShell cmdlet、Azure CLI 或 Azure Monitor REST API 使用诊断设置来启用存储。

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>存储帐户中指标和诊断日志的架构

设置指标和诊断日志集合后，当第一行数据可用时，将在你选择的存储帐户中创建一个存储容器。 这些 blob 的结构为：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
或者使用更简单的形式：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

例如，所有指标的 blob 名称可能是：

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

如果要记录弹性池中的数据，blob 名称则有所不同：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>从存储下载指标和日志

了解如何[从存储下载指标和诊断日志](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application)。

## <a name="data-retention-policy-and-pricing"></a>数据保留策略和定价

如果选择事件中心或存储帐户，可以指定保留策略。 此策略删除早于选定时间段的数据。 如果指定 Log analytics，保留策略将取决于所选的定价层。 超过每个月分配的免费数据引入单位会消耗诊断遥测。 提供的免费数据引入单位每月可免费监控多个数据库。 请注意，与空闲数据相比，工作负载较重的活动数据库越多，将引入的数据越多。 有关详细信息，请参阅 [Log Analytics 定价](https://azure.microsoft.com/pricing/details/monitor/)。 

如果使用 Azure SQL Analytics，则可以选择 Azure SQL Analytics 导航菜单上的 OMS 工作区，然后选择使用情况和预估成本，来轻松监视解决方案中的数据引入消耗量。

## <a name="metrics-and-logs-available"></a>可用的指标和日志

收集的监视遥测数据可用于你自己的**自定义分析**，并结合 [SQL Analytics 语言](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries)用于**应用程序开发**。 下面列出了所收集数据、指标和日志的结构。

## <a name="all-metrics"></a>所有指标

### <a name="all-metrics-for-elastic-pools"></a>弹性池的所有指标

|**资源**|**指标**|
|---|---|
|弹性池|eDTU 百分比、已用 eDTU、eDTU 限制、CPU 百分比、物理数据读取百分比、日志写入百分比、会话百分比、辅助角色百分比、存储、存储百分比、存储限制、XTP存储百分比 |

### <a name="all-metrics-for-azure-sql-database"></a>Azure SQL 数据库的所有指标

|**资源**|**指标**|
|---|---|
|Azure SQL 数据库|DTU 百分比、已用 DTU、DTU 限制、CPU 百分比、物理数据读取百分比、日志写入百分比、成功/失败/防火墙阻止的连接数、会话百分比、辅助角色百分比、存储、存储百分比、XTP 存储百分比和死锁 |

## <a name="logs"></a>日志

### <a name="logs-for-managed-instance"></a>托管实例的日志

### <a name="resource-usage-stats"></a>资源使用情况统计

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC]|记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：ResourceUsageStats|
|资源|资源的名称。|
|ResourceType|资源类型的名称。 始终是：MANAGEDINSTANCES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|托管实例的名称。|
|ResourceId|资源 URI。|
|SKU_s|托管实例产品 SKU|
|virtual_core_count_s|可用 vCore 的数目|
|avg_cpu_percent_s|CPU 平均百分比|
|reserved_storage_mb_s|托管实例上的保留存储容量|
|storage_space_used_mb_s|托管实例上的已使用存储|
|io_requests_s|IOPS 计数|
|io_bytes_read_s|已读取的 IOPS 字节数|
|io_bytes_written_s|已写入的 IOPS 字节数|

### <a name="logs-for-azure-sql-database-and-managed-instance-database"></a>Azure SQL 数据库和托管实例数据库的日志

### <a name="query-store-runtime-statistics"></a>查询数据存储运行时统计信息

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC]|记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：QueryStoreRuntimeStatistics|
|OperationName|操作的名称。 始终是：QueryStoreRuntimeStatisticsEvent|
|资源|资源的名称。|
|ResourceType|资源类型的名称。 始终是：SERVERS/DATABASES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|数据库所属的服务器的名称。|
|ElasticPoolName_s|数据库所属的弹性池的名称（如果存在）。|
|DatabaseName_s|数据库的名称。|
|ResourceId|资源 URI。|
|query_hash_s|查询哈希。|
|query_plan_hash_s|查询计划哈希。|
|statement_sql_handle_s|语句 SQL 句柄。|
|interval_start_time_d|自 1900-1-1 以时钟周期数开始 datetimeoffset 的时间间隔。|
|interval_end_time_d|自 1900-1-1 以时钟周期数结束 datetimeoffset 的时间间隔。|
|logical_io_writes_d|逻辑 IO 写入总次数。|
|max_logical_io_writes_d|每次执行逻辑 IO 写入的最大次数。|
|physical_io_reads_d|物理 IO 读取总次数。|
|max_physical_io_reads_d|每次执行逻辑 IO 读取的最大次数。|
|logical_io_reads_d|逻辑 IO 读取总次数。|
|max_logical_io_reads_d|每次执行逻辑 IO 读取的最大次数。|
|execution_type_d|执行类型。|
|count_executions_d|执行查询的次数。|
|cpu_time_d|查询使用的总 CPU 时间（以微秒为单位）。|
|max_cpu_time_d|单个执行消耗的最大 CPU 时间（以微秒为单位）。|
|dop_d|并行度总和。|
|max_dop_d|用于单个执行的最大并行度。|
|rowcount_d|返回的总行数。|
|max_rowcount_d|单个执行中返回的最大行数。|
|query_max_used_memory_d|已使用的内存总量（以 KB 为单位）。|
|max_query_max_used_memory_d|单个执行使用的最大内存量（以 KB 为单位）。|
|duration_d|总执行时间（以微秒为单位）。|
|max_duration_d|单个执行的最大执行时间。|
|num_physical_io_reads_d|总物理读取次数。|
|max_num_physical_io_reads_d|每次执行最大物理读取次数。|
|log_bytes_used_d|使用的日志字节总量。|
|max_log_bytes_used_d|每次执行使用的日志字节最大数量。|
|query_id_d|查询存储中查询的 ID。|
|plan_id_d|查询存储中计划的 ID。|

详细了解[查询存储运行时统计信息数据](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql)。

### <a name="query-store-wait-statistics"></a>查询存储等待统计信息

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC]|记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：QueryStoreWaitStatistics|
|OperationName|操作的名称。 始终是：QueryStoreWaitStatisticsEvent|
|资源|资源名称|
|ResourceType|资源类型的名称。 始终是：SERVERS/DATABASES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|数据库所属的服务器的名称。|
|ElasticPoolName_s|数据库所属的弹性池的名称（如果存在）。|
|DatabaseName_s|数据库的名称。|
|ResourceId|资源 URI。|
|wait_category_s|等待的类别。|
|is_parameterizable_s|查询是否可以参数化。|
|statement_type_s|语句的类型。|
|statement_key_hash_s|语句密钥哈希。|
|exec_type_d|执行的类型。|
|total_query_wait_time_ms_d|关于具体等待类别的查询的总等待时间。|
|max_query_wait_time_ms_d|单个执行中针对具体等待类别的查询的最大等待时间。|
|query_param_type_d|0|
|query_hash_s|查询存储中的查询哈希。|
|query_plan_hash_s|查询存储中的查询计划哈希。|
|statement_sql_handle_s|查询存储中的语句句柄。|
|interval_start_time_d|自 1900-1-1 以时钟周期数开始 datetimeoffset 的时间间隔。|
|interval_end_time_d|自 1900-1-1 以时钟周期数结束 datetimeoffset 的时间间隔。|
|count_executions_d|执行查询的次数。|
|query_id_d|查询存储中查询的 ID。|
|plan_id_d|查询存储中计划的 ID。|

详细了解[查询存储等待统计数据](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql)。

### <a name="errors-dataset"></a>错误数据集

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC]|记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：Errors|
|OperationName|操作的名称。 始终是：ErrorEvent|
|资源|资源名称|
|ResourceType|资源类型的名称。 始终是：SERVERS/DATABASES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|数据库所属的服务器的名称。|
|ElasticPoolName_s|数据库所属的弹性池的名称（如果存在）。|
|DatabaseName_s|数据库的名称。|
|ResourceId|资源 URI。|
|消息|纯文本格式的错误消息。|
|user_defined_b|用户定义位错误。|
|error_number_d|错误代码。|
|严重性|错误的严重性。|
|state_d|错误的状态。|
|query_hash_s|失败查询的查询哈希（如果存在）。|
|query_plan_hash_s|失败查询的查询计划哈希（如果存在）。|

详细了解 [SQL Server 错误消息](https://msdn.microsoft.com/library/cc645603.aspx)。

### <a name="database-wait-statistics-dataset"></a>数据库等待统计数据集

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC]|记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：DatabaseWaitStatistics|
|OperationName|操作的名称。 始终是：DatabaseWaitStatisticsEvent|
|资源|资源名称|
|ResourceType|资源类型的名称。 始终是：SERVERS/DATABASES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|数据库所属的服务器的名称。|
|ElasticPoolName_s|数据库所属的弹性池的名称（如果存在）。|
|DatabaseName_s|数据库的名称。|
|ResourceId|资源 URI。|
|wait_type_s|等待类型的名称。|
|start_utc_date_t [UTC]|测量期间开始时间。|
|end_utc_date_t [UTC]|测量期间结束时间。|
|delta_max_wait_time_ms_d|每次执行最大等待时间|
|delta_signal_wait_time_ms_d|总信号等待时间。|
|delta_wait_time_ms_d|期间内的总等待时间。|
|delta_waiting_tasks_count_d|等待任务数。|

详细了解[数据库等待统计信息](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql)。

### <a name="time-outs-dataset"></a>超时数据集

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC]|记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：Timeouts|
|OperationName|操作的名称。 始终是：TimeoutEvent|
|资源|资源名称|
|ResourceType|资源类型的名称。 始终是：SERVERS/DATABASES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|数据库所属的服务器的名称。|
|ElasticPoolName_s|数据库所属的弹性池的名称（如果存在）。|
|DatabaseName_s|数据库的名称。|
|ResourceId|资源 URI。|
|error_state_d|错误状态代码。|
|query_hash_s|查询哈希（如果存在）。|
|query_plan_hash_s|查询计划哈希（如果存在）。|

### <a name="blockings-dataset"></a>阻塞数据集

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC]|记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：Blocks|
|OperationName|操作的名称。 始终是：BlockEvent|
|资源|资源名称|
|ResourceType|资源类型的名称。 始终是：SERVERS/DATABASES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|数据库所属的服务器的名称。|
|ElasticPoolName_s|数据库所属的弹性池的名称（如果存在）。|
|DatabaseName_s|数据库的名称。|
|ResourceId|资源 URI。|
|lock_mode_s|查询所使用的锁定模式。|
|resource_owner_type_s|锁所有者。|
|blocked_process_filtered_s|阻止的进程报告 XML。|
|duration_d|锁定持续时间（以毫秒为单位）。|

### <a name="deadlocks-dataset"></a>死锁数据集

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC] |记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：Deadlocks|
|OperationName|操作的名称。 始终是：DeadlockEvent|
|资源|资源的名称。|
|ResourceType|资源类型的名称。 始终是：SERVERS/DATABASES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|数据库所属的服务器的名称。|
|ElasticPoolName_s|数据库所属的弹性池的名称（如果存在）。|
|DatabaseName_s|数据库的名称。 |
|ResourceId|资源 URI。|
|deadlock_xml_s|死锁报告 XML。|

### <a name="automatic-tuning-dataset"></a>自动优化数据集

|属性|Description|
|---|---|
|TenantId|租户 ID。|
|SourceSystem|始终是：Azure|
|TimeGenerated [UTC]|记录日志时的时间戳。|
|类型|始终是：AzureDiagnostics|
|ResourceProvider|资源提供程序的名称。 始终是：MICROSOFT.SQL|
|类别|类别的名称。 始终是：AutomaticTuning|
|资源|资源的名称。|
|ResourceType|资源类型的名称。 始终是：SERVERS/DATABASES|
|SubscriptionId|数据库所属的订阅 GUID。|
|resourceGroup|数据库所属的资源组的名称。|
|LogicalServerName_s|数据库所属的服务器的名称。|
|LogicalDatabaseName_s|数据库的名称。|
|ElasticPoolName_s|数据库所属的弹性池的名称（如果存在）。|
|DatabaseName_s|数据库的名称。|
|ResourceId|资源 URI。|
|RecommendationHash_s|自动优化建议的唯一哈希值。|
|OptionName_s|自动优化操作。|
|Schema_s|数据库架构。|
|Table_s|受影响的表。|
|IndexName_s|索引名称。|
|IndexColumns_s|列名称。|
|IncludedColumns_s|包括的列。|
|EstimatedImpact_s|自动优化建议 JSON 的估计影响。|
|Event_s|自动优化事件的类型。|
|Timestamp_t|上次更新时间戳。|

### <a name="intelligent-insights-dataset"></a>智能见解数据集
详细了解 [Intelligent Insights 日志格式](sql-database-intelligent-insights-use-diagnostics-log.md)。

## <a name="next-steps"></a>后续步骤

若要了解如何启用日志记录并了解各种 Azure 服务支持的指标和日志类别，请阅读以下主题：

 * [Microsoft Azure 中的指标概述](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
 * [Azure 诊断日志概述](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

若要了解事件中心，请阅读以下主题：

* [什么是 Azure 事件中心？](../event-hubs/event-hubs-what-is-event-hubs.md)
* [事件中心入门](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

若要详细了解存储，请参阅如何[从存储下载指标和诊断日志](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application)。
