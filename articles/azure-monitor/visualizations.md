---
title: 可视化 Azure Monitor 中的数据 | Microsoft 文档
description: 简要介绍可视化 Azure Monitor 中所存储数据（包括来自指标存储和 Log Analytics 中的数据）的可用方法。
author: bwren
manager: carmonm
editor: ''
services: azure-monitor
documentationcenter: azure-monitor
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2018
ms.author: bwren
ms.openlocfilehash: 4200accee94840a8a4455308f24a857fd5d7034a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632970"
---
# <a name="visualizing-data-from-azure-monitor"></a>可视化 Azure Monitor 中的数据
本文概述可视化 Azure Monitor 中所存储数据的可用方法。 具体数据包括 [Azure 指标存储中的指标](../azure-monitor/platform/data-collection.md#metrics)和 [Log Analytics 中的日志数据](../azure-monitor/platform/data-collection.md#logs)。 

图表和图形等可视化效果有助于你分析监视数据，从而深入了解问题并识别模式。 根据使用的工具，你还可以选择与组织内外的其他用户共享可视化效果。

## <a name="azure-dashboards"></a>Azure 仪表板
[Azure 仪表板](../azure-portal/azure-portal-dashboards.md)是 Azure 的主要仪表板技术。 这种在 Azure 基础结构和服务上提供的单一管理平台，在实现快速识别重要问题方面尤其有用。

![仪表板](media/visualizations/dashboard.png)

### <a name="advantages"></a>优点
- 深度集成到 Azure 中。 可视化效果可以固定到多个 Azure 页面的仪表板，包括指标资源管理器、Log Analytics 和 Application Insights。
- 支持指标和日志。
- 合并来自多个源的数据，包括来自[指标资源管理器](../monitoring-and-diagnostics/monitoring-metric-charts.md)、[Log Analytics 查询](../log-analytics/log-analytics-queries.md)及 Application Insights 中[映射](../application-insights/app-insights-app-map.md)和[可用性]()的输出。
- 包含个人或共享仪表板的选项。 与 Azure [基于角色的身份验证 (RBAC)](../role-based-access-control/overview.md) 集成。
- 自动刷新。 指标刷新的时间范围至少为 5 分钟。 以 1 分钟为间隔刷新日志。
- 带有时间戳和自定义参数的参数化指标仪表板。
- 灵活的布局选项。
- 全屏模式。


### <a name="limitations"></a>限制
- 有限控制不支持数据表的 Log Analytics 可视化效果。 数据系列总数限制为 10，其他数据系列在“其他” Bucket 下进行分组。
- 无自定义参数支持 Log Analytics 图表。
- Log Analytics 图表仅限于过去 30 天。
- Log Analytics 图表只能固定到共享仪表板。
- 没有与仪表板数据的交互操作。
- 上下文向下钻取受限。

## <a name="azure-monitor-views"></a>Azure Monitor 视图
借助 [Azure Monitor 中的视图](../azure-monitor/platform/view-designer.md)，可以使用 Log Analytics 中存储的日志数据创建自定义可视化效果。 它们由[监视解决方案](../azure-monitor/insights/solutions.md)用于呈现所收集的数据。

![查看](media/visualizations/view.png)

### <a name="advantages"></a>优点
- Log Analytics 数据丰富的可视化效果。
- 导出和导入视图可以将其传输到其他资源组和订阅。
- 通过工作区和监视解决方案集成到 Log Analytic 管理模型中。
- 用于自定义参数的[筛选器](../azure-monitor/platform/view-designer-filters.md)。
- 具有交互性，支持多级钻取（一个视图钻取到另一个视图）

### <a name="limitations"></a>限制
- 支持日志，但不支持指标。
- 无个人视图。 适用于有权限访问工作区的所有用户。
- 无自动刷新。
- 布局选项有限。
- 不支持跨 Log Analytics 工作区和 Application Insights 应用程序进行查询。
- 查询的响应大小限制为 8MB，查询的执行时间限制为 110 秒。



## <a name="application-insights-workbooks"></a>Application Insights 工作簿
[工作簿](../application-insights/app-insights-usage-workbooks.md)是交互式文档，可以让用户深入了解团队内部的数据、调查和协作。 例如，工作簿在故障排查指南和事件分析方面非常有用。

![工作簿](media/visualizations/workbook.png)

### <a name="advantages"></a>优点
- 支持指标和日志。
- 支持启用交互式报表的参数，其中，选择表中的元素将以动态方式更新关联的图表和可视化效果。
- 类似于文档的流。
- 包含个人或共享工作簿的选项。
- 轻松、易协作的创作体验。
- 模板支持基于 GitHub 的公共模板库。

### <a name="limitations"></a>限制
- 无自动刷新。
- 没有类似仪表板的密集布局，这使得工作簿作为单一管理平台不太有用。 旨在提供更深入的见解。


## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) 对于创建以业务为中心的仪表板和报表，以及用于分析长期 KPI 趋势的报表特别有用。 你可以将 [Log Analytics 查询结果导入 Power BI 数据集](../log-analytics/log-analytics-powerbi.md)，以便利用其各项功能，例如合并不同来源的数据和在 Web 和移动设备上共享报表。

![Power BI](media/visualizations/power-bi.png)

### <a name="advantages"></a>优点
- 丰富的可视化效果。
- 广泛的交互性，包括放大和交叉筛选。
- 易于在整个组织内共享。
- 与来自多个数据源的其他数据集成。
- 结果缓存在多维数据集中，性能更佳。


### <a name="limitations"></a>限制
- 支持日志，但不支持指标。
- 无 Azure 集成。 无法通过 Azure 资源管理器管理仪表板和模型。
- 需要将查询结果导入到 Power BI 模型中进行配置。 结果大小和刷新存在限制。
- 每天的数据刷新次数限制为 8 次。


## <a name="grafana"></a>Grafana
[Grafana](https://grafana.com/) 是一个适用于操作仪表板的开放平台。 它尤其适用于检测、隔离和归类操作事件。 你可以将 [Grafana Azure Monitor 数据源插件](../monitoring-and-diagnostics/monitor-send-to-grafana.md)添加到 Azure 订阅中，使其可视化 Azure 指标数据。

![Grafana](media/visualizations/grafana.png)

### <a name="advantages"></a>优点
- 丰富的可视化效果。
- 丰富的数据源生态系统。
- 数据交互功能，包括放大。
- 支持参数。

### <a name="limitations"></a>限制
- 支持指标，但不支持日志。
- 无 Azure 集成。 无法通过 Azure 资源管理器管理仪表板和模型。
- 支持额外 Grafana 基础结构的成本或用于 Grafana Cloud 的额外成本。


## <a name="build-your-own-custom-application"></a>生成自己的自定义应用程序
你可以使用任何 REST 客户端通过其 API 访问 Azure 指标和 Log Analytics 中的数据，从而可以生成自己的自定义网站和应用程序。

### <a name="advantages"></a>优点
- 在 UI、可视化效果、交互性和功能方面具有完全的灵活性。
- 可将指标和日志数据与其他数据源相结合。

### <a name="disadvantages"></a>缺点
- 所需的工程工作量明显较大。


## <a name="next-steps"></a>后续步骤
- 了解[由 Azure Monitor 收集的数据](../azure-monitor/platform/data-collection.md)。
- 了解 [Azure 仪表板](../azure-portal/azure-portal-dashboards.md)。
- 了解 [Azure Monitor 中的视图](../azure-monitor/platform/view-designer.md)。
- 了解 [Application Insights 中的工作簿](../application-insights/app-insights-usage-workbooks.md)。
- 了解如何[将日志数据导入 Power BI](../log-analytics/log-analytics-powerbi.md)。
- 了解 [Grafana Azure Monitor 数据源插件](../monitoring-and-diagnostics/monitor-send-to-grafana.md)。
