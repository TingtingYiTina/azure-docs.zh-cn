---
title: 充分利用 Azure Application Insights | Microsoft Docs
description: 开始使用 Application Insights 后，下面概述了可以浏览的功能。
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: a1244076efe1c920a23f29de9d25ab6845747fe4
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245845"
---
# <a name="more-telemetry-from-application-insights"></a>Application Insights 中的更多遥测
[将 Application Insights 添加到 ASP.NET 代码](app-insights-asp-net.md)后，可执行一些操作来获取更多遥测。 

| 操作 | 用户所得|
|---|---|
|（IIS 服务器）在每台服务器计算机上[安装状态监视器](https://go.microsoft.com/fwlink/?LinkId=506648)。<br/>（Azure Web 应用）在 Web 应用的 Azure 控制面板中，打开“Application Insights”边栏选项卡。| [**性能计数器**](app-insights-performance-counters.md)<br/>[**异常**](app-insights-asp-net-exceptions.md) - 详细的堆栈跟踪<br/>[**依赖项**](app-insights-asp-net-dependencies.md)|
|[将 JavaScript 代码片段添加到网页。](app-insights-javascript.md)|[页面性能](app-insights-usage-overview.md)，浏览器异常，AJAX 性能。 自定义客户端遥测。|
|[创建可用性 Web 测试](app-insights-monitor-web-app-availability.md)|当站点不可用时接收警报|
|[确保 buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) 由 MSBuild 生成|[生成批注指标图表](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[写入自定义事件和指标](app-insights-api-custom-events-metrics.md)|对业务活动和指标进行计数，跟踪详细使用情况等。|
|[分析实时站点](https://aka.ms/AIProfilerPreview)|详细的实时 Web 应用的功能计时|






