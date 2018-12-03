---
title: Log Analytics 用户从 OMS 门户转到 Azure 门户的常见问题 | Microsoft Docs
description: Log Analytics 用户从 OMS 门户转到 Azure 门户的常见问题解答。
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 5f5143800c423bdbaac9994b55e500723018dce4
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634347"
---
# <a name="common-questions-for-transition-from-oms-portal-to-azure-portal-for-log-analytics-users"></a>Log Analytics 用户从 OMS 门户转到 Azure 门户的常见问题
Log Analytics 一开始使用其自己的门户（称为 OMS 门户）来管理其配置并分析收集的数据。  此门户的所有功能已移至 Azure 门户，在其中继续进行开发。

本文解答了用户进行此转换的常见问题。  如果你在 OMS 门户中使用过 Log Analytics，并且想要在 Azure 门户中执行相同的任务，则可在此处查找答案。

## <a name="do-i-need-to-migrate-anything"></a>我是否需要迁移什么内容？
不是。 Log Analytics 本身并没有产生更改，所以不需要迁移任何内容。 唯一更改的是用于访问 Log Analytics 的接口。 事实上，现在可以使用 Azure 门户来访问在 OMS 门户中所使用的工作区、解决方案、视图和日志搜索内容。

## <a name="where-do-i-find-log-analytics-in-azure"></a>在 Azure 中的何处查找 Log Analytics？
通过 [https://portal.azure.com](https://portal.azure.com) 登录到 Azure 门户。  单击“所有服务”，然后在资源列表中键入“Log Analytics”。 选择“Log Analytics”，然后选择工作区。 此时会显示工作区的摘要页。

![Log Analytics 工作区](media/log-analytics-oms-portal-faq/log-analytics.png)

## <a name="how-do-i-manage-permissions"></a>如何管理权限？
如果无法访问 Azure 门户中的 Log Analytics 工作区，则需根据[基于 Azure 角色的访问权限](../role-based-access-control/role-assignments-portal.md)中的说明来配置权限。 有关如何管理工作区权限的详细信息，请参阅[管理工作区](../log-analytics/log-analytics-manage-access.md#manage-accounts-and-users)。 有关如何管理警报权限的信息，请参阅 [Azure Monitor 的角色、权限和安全入门](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md)。

## <a name="how-do-i-create-a-new-workspace"></a>如何创建新工作区？ 
在 Azure 门户的工作区列表中单击“添加”。  如需完整详细信息，请参阅[在 Azure 门户中创建 Log Analytics 工作区](../log-analytics/log-analytics-quick-create-workspace.md)。

![概述页](media/log-analytics-oms-portal-faq/new-workspace.png)

## <a name="where-is-my-overview-page"></a>概览页在哪里？
OMS 门户中的主屏幕显示在工作区中安装的所有管理解决方案的磁贴，以及已创建的任何自定义视图。 Azure 门户中提供了此相同的视图。 从工作区中选择“工作区摘要”。

![概述页](media/log-analytics-oms-portal-faq/overview.png)

## <a name="how-do-i-open-log-search-and-view-designer"></a>如何打开日志搜索和视图设计器？
“日志搜索”和“视图设计器”在 Azure 门户的主页和工作区的左侧菜单中均可用，位于“概览”右侧。

## <a name="where-do-i-find-settings"></a>在哪里可以找到我的设置？
OMS 门户的“设置”部分的许多设置在 Azure 门户的工作区的“高级设置”菜单中可用。

![高级设置](media/log-analytics-oms-portal-faq/advanced-settings.png)

以下部分提供了一个完整的列表，概述了如何访问以前在 OMS 门户的“设置”部分提供的设置。

### <a name="accounts"></a>帐户 
帐户设置在 Azure 门户的不同位置管理，如下表所述。

| OMS 门户中的设置 | Azure 门户中的等效项 |
|:---|:---|
| 自动化帐户 | 工作区的“自动化帐户”菜单。 |
| Azure 订阅和数据计划 | 工作区的“定价层”菜单。 |
| 管理用户 | 使用 Azure 基于角色的访问权限来[管理工作区的权限](#how-do-i-manage-permissions)。 |
| 工作区信息 | 信息在工作区的“OMS 工作区”菜单中提供。 |

### <a name="alerts"></a>警报
基于 Log Analytics 查询的警报规则现在在[统一警报体验](#how-do-i-create-and-manage-alerts)中管理。 

### <a name="computer-groups"></a>计算机组
在工作区的“高级设置”菜单中管理“计算机组”。 

### <a name="connected-sources"></a>连接的源
在工作区的“高级设置”菜单中管理大多数“连接的源”设置。 下表详细说明了此菜单的每个部分。

| OMS 门户中的设置 | Azure 门户中的等效项 |
|:---|:---|
| Windows Server   | 工作区的“高级设置”菜单。 |
| Linux 服务器   | 工作区的“高级设置”菜单。 |
| Azure 存储     | 工作区的“高级设置”菜单。 |
| System Center     | 工作区的“高级设置”菜单。 |
| Office 365        | 请参阅 [Office 365 管理解决方案的文档](../azure-monitor/insights/solution-office-365.md)，了解配置详细信息。 |
| Windows 遥测 | 解决方案的“设置”菜单。 有关配置详细信息，请参阅 [Azure 门户中的 Windows 分析](/windows/deployment/update/windows-analytics-azure-portal)。 |
| ITSM 连接器    | 请参阅[使用 IT 服务管理连接器连接 ITSM 产品/服务](../log-analytics/../azure-monitor/platform/itsmc-connections.md)，了解如何将 ITSM 服务与 Log Analytics 连接。 |

### <a name="data"></a>数据
在工作区的“高级设置”菜单中管理大多数“数据”设置。 下表详细说明了此菜单的每个部分。

| OMS 门户中的设置 | Azure 门户中的等效项 |
|:---|:---|
| Windows 事件日志           | 工作区的“高级设置”菜单。 |
| Windows 性能计数器 | 工作区的“高级设置”菜单。 |
| Linux 性能计数器   | 工作区的“高级设置”菜单。 |
| IIS Logs                     | 工作区的“高级设置”菜单。 |
| 自定义字段                | 工作区的“高级设置”菜单。 |
| 自定义日志                  | 工作区的“高级设置”菜单。 |
| Syslog                       | 工作区的“高级设置”菜单。 |
| Application Insights         | 由于 Log Analytics 和 Application Insights 共享同一数据引擎，因此此解决方案已弃用。  |
| Windows 文件跟踪        | Azure 自动化中的“更改跟踪”菜单。 有关详细信息，请参阅[使用更改跟踪解决方案跟踪环境中的更改](../automation/automation-change-tracking.md)。 |
| Windows 注册表跟踪        | Azure 自动化中的“更改跟踪”菜单。 有关详细信息，请参阅[使用更改跟踪解决方案跟踪环境中的更改](../automation/automation-change-tracking.md)。 |
| Linux 文件跟踪          | Azure 自动化中的“更改跟踪”菜单。 有关详细信息，请参阅[使用更改跟踪解决方案跟踪环境中的更改](../automation/automation-change-tracking.md)。 |

### <a name="solutions"></a>解决方案
管理工作区的“解决方案”菜单中的解决方案。 

## <a name="how-do-i-install-and-remove-management-solutions"></a>如何安装和删除管理解决方案？
在 OMS 门户中，请通过“解决方案”库安装管理解决方案，通过“设置”将其删除。 在 Azure 门户中，请从 Azure 市场[安装管理解决方案](../azure-monitor/insights/solutions.md#install-a-management-solution)。 请从已安装解决方案的列表中[删除解决方案](../azure-monitor/insights/solutions.md#remove-a-management-solution)。

## <a name="how-do-i-create-and-manage-alerts"></a>如何创建和管理警报？
基于 Log Analytics 查询的警报规则现在在[统一警报体验](../monitoring-and-diagnostics/alert-metric.md)中管理。 请参阅[如何将警报从 Log Analytics 扩展到 Azure 警报中](../monitoring-and-diagnostics/monitoring-alerts-extend-tool.md)，详细了解如何在 Azure 门户中配置和使用警报。

## <a name="how-do-i-access-my-dashboards"></a>如何访问我的仪表板？
Log Analytics 中的[我的仪表板](../log-analytics/../azure-monitor/platform/dashboards.md)功能已被弃用。 此功能允许你有一个专用的视图设计器部件集合，已被内置的 Azure 仪表板功能所取代。 你可以使用共享视图的[视图设计器](../log-analytics/../azure-monitor/platform/view-designer.md)继续可视化 Log Analytics 中的数据。 此外，还可以将这些视图或者[各个查询](../log-analytics/log-analytics-tutorial-dashboards.md)的可视化效果固定到 Azure 仪表板。

## <a name="how-do-i-check-my-usage"></a>如何查看我的使用情况？
现在可以轻松地查看和管理 Log Analytics 的使用情况和成本，只需在工作区中选择“使用情况和预估成本”即可。

![使用情况和预估成本](media/log-analytics-oms-portal-faq/usage.png)


## <a name="can-i-still-use-the-classic-portal"></a>是否仍可使用经典门户？
在有限的时间内，仍可通过以下 URL 使用你自己的工作区名称访问门户： https://\<你的工作区名称\>.portal.mms.microsoft.com。 不过，我们仍建议你使用 Azure 门户。如果碰到阻塞性问题，请通过 LAUpgradeFeedback@microsoft.com 向我们提供反馈。

## <a name="next-steps"></a>后续步骤

- 使用 Azure 门户[查找和安装管理解决方案](../azure-monitor/insights/solutions.md)。
- 了解 [Azure 门户中的“日志搜索”](log-analytics-log-search-portals.md)。
