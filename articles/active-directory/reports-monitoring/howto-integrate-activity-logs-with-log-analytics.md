---
title: 如何使用 Azure Monitor 将 Azure Active Directory 日志与 Log Analytics 集成（预览版） | Microsoft Docs
description: 了解如何使用 Azure Monitor 将 Azure Active Directory 日志与 Log Analytics 集成（预览版）
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 7509081bbf43aeaf39570f84afef81b6dd5a39fe
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621660"
---
# <a name="integrate-azure-ad-logs-with-log-analytics-using-azure-monitor-preview"></a>使用 Azure Monitor 将 Azure AD 日志与 Log Analytics 集成（预览版）

Log Analytics 允许你跨各种数据源查询数据以查找特定事件、分析趋势和执行关联。 通过将 Azure AD 活动与 Log Analytics 集成，你现在可以执行以下任务：

 * 比较 Azure AD 登录日志与 Azure 安全中心发布的安全日志

 * 通过从 Azure Application Insights 关联应用程序性能数据，可以解决应用程序登录页上的性能瓶颈。  

Ignite 会话中的以下视频通过实际用户方案演示了将 Log Analytics 用于 Azure AD 日志的优点。

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

本文介绍如何使用 Azure Monitor 将 Azure Active Directory (Azure AD) 日志与 Log Analytics 集成。

## <a name="supported-reports"></a>支持的报表

你可以将审核活动日志和登录活动日志路由到 Log Analytics 以供进一步分析。 

* **审核日志**: 可以通过[审核日志活动报表](concept-audit-logs.md)访问在租户中执行的每个任务的历史记录。
* **登录日志**：可以通过[登录活动报表](concept-sign-ins.md)来确定谁执行了审核日志中报告的任务。

> [!NOTE]
> 目前不支持 B2C 相关的审核和登录活动日志。
>

## <a name="prerequisites"></a>先决条件 

若要使用此功能，需满足以下条件：

* Azure 订阅。 如果没有 Azure 订阅，可以[注册免费试用版](https://azure.microsoft.com/free/)。
* Azure AD 租户。
* 一个是 Azure AD 租户的全局管理员或安全管理员的用户。
* 在 Azure 订阅中创建 Log Analytics 工作区。 了解如何[创建 Log Analytics 工作区](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace)。

## <a name="send-logs-to-log-analytics"></a>将日志发送到 Log Analytics

1. 登录到 [Azure 门户](https://portal.azure.com)。 

2. 选择“Azure Active Directory” > “诊断设置” -> “添加诊断设置”。 还可以从“审核日志”或“登录”页选择“导出设置”，以转到诊断设置配置页。  
    
3. 在“诊断设置”菜单中，选中“发送到 Log Analytics”复选框，并选择“配置”。

4. 选择要将日志发送到的 Log Analytics 工作区，或在提供的对话框中创建新的工作区。  

5. 执行下列两项操作或之一：
    * 若要将审核日志发送到 Log Analytics 工作区，请选中“AuditLogs”复选框。 
    * 若要将登录日志发送到 Log Analytics 工作区，请选中“SignInLogs”复选框。

6. 选择“保存”，保存设置。

    ![诊断设置](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. 大约 15 分钟后，验证事件是否已流式传输到 Log Analytics 工作区。

## <a name="next-steps"></a>后续步骤

* [在 Log Analytics 中分析 Azure AD 活动日志](howto-analyze-activity-logs-log-analytics.md)
* [安装和使用用于 Azure Active Directory 的 Log Analytics 视图](howto-install-use-log-analytics-views.md)
