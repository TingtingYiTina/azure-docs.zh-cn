---
title: Azure 安全中心的警报验证 | Microsoft Docs
description: 本文档介绍了如何在 Azure 安全中心验证安全警报。
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2018
ms.author: rkarlin
ms.openlocfilehash: 2c0bb2a68eaaa8183463efbdc2848567ab67d1b9
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619738"
---
# <a name="alerts-validation-in-azure-security-center"></a>Azure 安全中心的警报验证
本文档介绍如何验证系统是否已针对 Azure 安全中心警报进行了适当的配置。

## <a name="what-are-security-alerts"></a>什么是安全警报？
安全中心从 Azure 资源、网络和已连接的合作伙伴解决方案（例如防火墙和终结点保护解决方案）自动收集、分析并整合日志数据，以检测威胁并发出警报。 有关安全警报的详细信息，请阅读[管理和响应 Azure 安全中心的安全警报](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)；若要详细了解不同类型的警报，请阅读[了解 Azure 安全中心中的安全警报](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)。

## <a name="alert-validation"></a>警报验证
在计算机上安装安全中心代理以后，请在需要让其充当与警报相关的受攻击资源的计算机中执行以下步骤：

1. 将一个可执行文件（例如 calc.exe）复制到计算机的桌面或方便操作的其他目录。
2. 将该文件重命名为 ASC_AlertTest_662jfi039N.exe。
3. 打开命令提示符，使用一个参数（假参数名称即可）执行该文件，例如：ASC_AlertTest_662jfi039N.exe -foo
4. 等待 5 到 10 分钟，然后打开安全中心警报。 其中会显示类似以下内容的警报：

    ![警报验证](./media/security-center-alert-validation/security-center-alert-validation-fig2.png)

查看此警报时，请确保“启用参数审核”字段显示为 true。 如果显示 false，则需启用命令行参数审核。 可以使用以下命令行启用该选项：

reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"


> [!NOTE]
> 观看 [Azure 安全中心内的警报验证](https://channel9.msdn.com/Blogs/Azure-Security-Videos/Alert-Validation-in-Azure-Security-Center)视频，查看此功能的演示。

## <a name="see-also"></a>另请参阅
本文介绍了警报验证过程。 熟悉该验证以后，请尝试阅读以下文章：

* [Managing and responding to security alerts in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)（管理和响应 Azure 安全中心的安全警报）。 了解如何管理警报并响应安全中心的安全事件。
* [Security health monitoring in Azure Security Center](security-center-monitoring.md)（在 Azure 安全中心进行安全运行状况监视）。 了解如何监视 Azure 资源的运行状况。
* [了解 Azure 安全中心中的安全警报](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)。 了解不同类型的安全警报。
* [Azure 安全中心故障排除指南](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide)。 了解如何排查安全中心的常见问题。
* [Azure Security Center FAQ](security-center-faq.md)（Azure 安全中心常见问题）。 查找有关如何使用服务的常见问题。
* [Azure 安全性博客](https://blogs.msdn.com/b/azuresecurity/)。 查找关于 Azure 安全性及合规性的博客文章。
