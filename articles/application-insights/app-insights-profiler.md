---
title: 使用 Application Insights 探查实时 Azure Web 应用 | Microsoft Docs
description: 使用 Application Insights Profiler 探查 Azure 上的实时 Web 应用
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 9aae08aa5906f341a890ac15e30d2863109d83a2
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140002"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>使用 Application Insights 探查实时 Azure Web 应用

Profiler 目前适用于 Web 应用上运行的 ASP.NET 和 ASP.NET Core Web 应用。 若要使用 Profiler，“基本”服务层或更高层是必需的。

## <a id="installation"></a> 为 Web 应用启用 Profiler
若要为 Web 应用启用 Profiler，请遵循下面的说明。 如果你在运行另一种类型的 Azure 服务，则下面提供了用于在其他受支持平台上启用 Profiler 的说明：
* [云服务](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric 应用程序](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [虚拟机](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


Application Insights Profiler 是随 Application Insights 站点扩展一起安装的。 你需要安装站点扩展并对其进行配置以获取你的 Azure Web 应用的配置文件。 部署 Web 应用后，即使在源代码中包括了 App Insights SDK，也要执行以下步骤来启用 Profiler。

1. 转到 Azure 门户中的“应用服务”窗格。
1. 导航到“设置”>“监视”窗格。

   ![在应用服务门户上启用 App Insights](./media/app-insights-profiler/AppInsights-AppServices.png)

1. 按窗格中的说明创建新资源，或者选择现有的 App Insights 资源，以便监视 Web 应用。 接受所有默认选项。 默认情况下会启用**代码级诊断**，并且它会启用 Profiler。

   ![添加 App Insights 站点扩展][Enablement UI]

1. Profiler 现在已随 App Insights 站点扩展安装，并且使用应用服务应用设置启用。

    ![Profiler 的应用设置][profiler-app-setting]

## <a name="disable-profiler"></a>禁用 Profiler

若要为单个 Web 应用实例停止或重启 Profiler，请在“Web 作业”下转到“Web 应用资源”。 若要删除 Profiler，请转到“扩展”。

![禁用针对 Web 作业的 Profiler][disable-profiler-webjob]

建议在所有 Web 应用上都启用 Profiler，以便尽早发现任何性能问题。

如果使用 WebDeploy 将更改部署到 Web 应用程序，请确保排除 App_Data 文件夹，以防在部署期间将它删除。 否则，下一次将 Web 应用程序部署到 Azure 时，会删除 Profiler 扩展文件。



## <a name="next-steps"></a>后续步骤

* [在 Visual Studio 中使用 Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png

