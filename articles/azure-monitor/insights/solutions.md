---
title: Azure 中的管理解决方案 | Microsoft Docs
description: Azure 中的管理解决方案是逻辑、可视化效果和数据采集规则的集合，提供围绕特定问题领域制定的指标。  本文提供有关安装和使用管理解决方案的信息。
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: f056e30850168ec8a9179e1e11686f7221f6fded
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713880"
---
# <a name="management-solutions-in-azure"></a>Azure 中的管理解决方案
管理解决方案利用 Azure 中的服务来提供特定应用程序或服务的更多操作见解。 本文简要概述 Azure 中的管理解决方案，并详细介绍如何使用和安装这些解决方案。

管理解决方案通常将信息收集到 Log Analytics 中，并提供日志搜索和视图用于分析收集的数据。 这些解决方案还可以利用 Azure 自动化等其他服务来执行与应用程序或服务相关的操作。

可将管理解决方案添加到所用任何应用程序和服务的 Azure 订阅。 这些解决方案是免费提供的，但收集数据可能会产生使用费。 除了由 Microsoft 提供的解决方案以外，合作伙伴和客户还可以[创建管理解决方案](solutions-creating.md)，以便在各自环境中使用或通过社区提供给客户使用。

## <a name="using-management-solutions"></a>使用管理解决方案
每个 Log Analytics 工作区的“概述”页针对该工作区中安装的每个解决方案显示一个磁贴。 单击解决方案的磁贴会打开其视图，其中包含该解决方案收集的数据的更详细分析。

![概述](media/solutions/overview.png)

管理解决方案可以包含多种类型的 Azure 资源。可以像查看其他任何资源一样查看解决方案包含的任何资源。 例如，解决方案中包含的任何日志搜索显示在工作区的“保存的搜索”中。 在 Log Analytics 中执行即席分析时，可以使用这些搜索。

## <a name="list-installed-management-solutions"></a>列出已安装的管理解决方案 
使用以下过程列出订阅中安装的管理解决方案。

1. 登录到 Azure 门户。
2. 在左窗格中，选择“所有服务”。
3. 向下滚动到“**解决方案**”，或者在“**筛选器**”对话框中键入 *solutions*。
4. 将列出所有工作区中安装的解决方案。 解决方案名称的后面是安装该解决方案的 Log Analytics 工作区的名称。
1. 使用屏幕顶部的下拉框可按订阅或资源组进行筛选。


![列出所有解决方案](media/solutions/list-solutions-all.png)

单击某个解决方案的名称打开其摘要页。 此页显示解决方案中包含的所有 Log Analytics 视图，并提供解决方案本身及其工作区的不同选项。 使用上述过程之一查看解决方案的摘要页以列出解决方案，然后单击解决方案的名称。

![解决方案属性](media/solutions/solution-properties.png)



## <a name="install-a-management-solution"></a>安装管理解决方案
[Azure 市场](https://azuremarketplace.microsoft.com)中提供了 Microsoft 和合作伙伴的管理解决方案。 可以搜索可用的解决方案，并使用以下过程进行安装。

1. 在[订阅的解决方案列表](#list-installed-management-solutions)中，单击“添加”。 
1. 在“管理解决方案”的右侧，单击“更多”。 
1. 找到所需的管理解决方案并阅读其说明。
1. 单击“创建”以启动安装进程。
1. 安装过程开始后，系统会提示提供所需的配置（根据每个解决方案而异）。 所有配置都要求选择要在其中安装该解决方案的 Log Analytics 工作区，以及要将解决方案数据收集到的位置。 

![安装解决方案](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>从社区安装解决方案
社区成员可以将管理解决方案提交到 Azure 快速入门模板。 可以直接安装这些解决方案，或者下载模板，以便今后安装。

1. 请遵循 [Log Analytics 工作区和自动化帐户](#log-analytics-workspace-and-automation-account)中所述的过程来链接工作区和帐户。
2. 转到 [Azure 快速入门模板](https://azure.microsoft.com/documentation/templates/)。 
3. 搜索感兴趣的解决方案。
4. 从结果中选择解决方案以查看其详细信息。
5. 单击“**部署到 Azure**”按钮。
6. 除了解决方案中任何参数的值，系统还会提示提供资源组和位置等信息。
7. 单击“**购买**”可安装解决方案。


## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics 工作区和自动化帐户
所有管理解决方案都需要使用一个 [Log Analytics 工作区](../../log-analytics/log-analytics-manage-access.md)来存储解决方案收集的数据，以及托管其日志搜索和视图。 某些解决方案还需要使用一个[自动化帐户](../../automation/automation-security-overview.md#automation-account-overview)来包含 Runbook 和相关资源。 工作区和帐户必须满足以下要求。

* 解决方案的每项安装只能使用一个 Log Analytics 工作区和一个自动化帐户。 可将解决方案单独安装到多个工作区。
* 如果解决方案需要自动化帐户，则必须将 Log Analytics 工作区和自动化帐户相互链接。 一个 Log Analytics 工作区只能链接到一个自动化帐户，而一个自动化帐户也只能链接到一个 Log Analytics 工作区。
* 若要进行链接，Log Analytics 工作区和自动化帐户必须位于相同的资源组和区域中。 美国东部区域的工作区以及美国东部 2 区的自动化帐户除外。

### <a name="creating-a-link-between-a-log-analytics-workspace-and-automation-account"></a>在 Log Analytics 工作区和自动化帐户之间创建链接
如何指定 Log Analytics 工作区和自动化帐户取决于解决方案的安装方法。

* 通过 Azure 市场安装解决方案时，系统会提示提供一个工作区和自动化帐户。 如果工作区与自动化帐户之间尚未建立链接，则系统会创建这种链接。
* 对于 Azure 市场外的解决方案，必须在安装解决方案之前链接 Log Analytics 工作区和自动化帐户。 为此，可以在 Azure 市场中选择任何解决方案，并选择 Log Analytics 工作区和自动化帐户。 无需实际安装解决方案，因为只要选择了 Log Analytics 工作区和自动化帐户，就会创建链接。 创建链接后，可以对任何解决方案使用该 Log Analytics 工作区和自动化帐户。

### <a name="verifying-the-link-between-a-log-analytics-workspace-and-automation-account"></a>验证 Log Analytics 工作区和自动化帐户之间的链接
可以使用以下过程验证 Log Analytics 工作区和自动化帐户之间的链接。

1. 在 Azure 门户中选择自动化帐户。
1. 滚动到菜单的“相关资源”部分。
1. 如果“工作区”设置已启用，则此帐户将链接到 Log Analytics 工作区。 可单击“工作区”查看工作区的详细信息。

## <a name="remove-a-management-solution"></a>删除管理解决方案
若要删除已安装的解决方案，请在[已安装的解决方案列表](#list-installed-management-solutions)中找到它。 单击解决方案的名称打开其摘要页，然后单击“删除”。




## <a name="next-steps"></a>后续步骤
* 获取 [Microsoft 提供的管理解决方案列表](solutions-inventory.md)。
* 了解如何[创建查询](../../log-analytics/log-analytics-queries.md)来分析管理解决方案收集的数据。

