---
title: 在 Azure 安全中心启用 VM 代理 | Microsoft 文档
description: 本文档演示如何实现 Azure 安全中心建议**启用 VM 代理**。
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 7a059cd5c22306c42adc4c7c8fcc4e52c3ebcd00
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52313565"
---
# <a name="enable-vm-agent-in-azure-security-center"></a>在 Azure 安全中心启用 VM 代理
必须在虚拟机 (VM) 上安装 VM 代理以便[启用数据收集](security-center-enable-data-collection.md)。  使用 Azure 安全中心，可以查看需要 VM 代理的 VM，并建议在这些 VM 上启用 VM 代理。

对于从 Azure 市场部署的 VM，默认安装 VM 代理。 文章 [VM 代理和扩展 - 第 2 部分](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)提供有关如何安装 VM 代理的信息。

> [!NOTE]
> 本文档将使用示例部署介绍该服务。 这并非一份循序渐进的指南。
>
>

## <a name="implement-the-recommendation"></a>实现该建议
1. 在“建议”边栏选项卡中，选择“启用 VM 代理”。
   ![启用 VM 代理][1]
2. 这会打开边栏选项卡“缺少 VM 代理或 VM 代理未响应”。 此边栏选项卡列出了需要 VM 代理的 VM。 按照边栏选项卡上的说明来安装 VM 代理。
   ![缺少 VM 代理][2]

## <a name="see-also"></a>另请参阅
若要了解有关安全中心的详细信息，请参阅以下文章：

* [在 Azure 安全中心中设置安全策略](security-center-azure-policy.md) - 了解如何配置 Azure 订阅和资源组的安全策略。
* [管理 Azure 安全中心安全建议](security-center-recommendations.md) - 了解安全建议如何有助于保护 Azure 资源。
* [Azure 安全中心的安全性运行状况监视](security-center-monitoring.md) - 了解如何监视 Azure 资源的运行状况。
* [管理和响应 Azure 安全中心的安全警报](security-center-managing-and-responding-alerts.md) - 了解如何管理和响应安全警报。
* [通过 Azure 安全中心监视合作伙伴解决方案](security-center-partner-solutions.md) - 了解如何监视合作伙伴解决方案的运行状态。
* [Azure 安全中心常见问题解答](security-center-faq.md)-- 查找有关使用服务的常见问题。
* [Azure 安全博客](https://blogs.msdn.com/b/azuresecurity/) - 获取最新的 Azure 安全新闻和信息。

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
