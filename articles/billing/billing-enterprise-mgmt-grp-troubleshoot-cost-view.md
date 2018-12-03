---
title: 排查 Azure 企业成本视图问题 | Microsoft Docs
description: 了解如何解决 Azure 门户中可能遇到的有关组织成本查看的任何问题。
author: rthorn17
manager: adpick
editor: ''
ms.assetid: ''
ms.service: billing
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2017
ms.author: cwatson
ms.openlocfilehash: be74714667c2fed57a797a972ddcd7949cae5933
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52274664"
---
# <a name="troubleshoot-enterprise-cost-views"></a>排查企业成本查看问题 

在企业注册中，存在多种可能导致注册中的用户无法查看成本的设置。  如果注册并非直接通过 Microsoft 购买，那么这些设置由注册管理员或合作伙伴管理。  本文可以帮助你了解具体设置以及它们对注册的影响。 这些设置独立于 [Azure RBAC 角色](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)。 


## <a name="enabling-access-to-costs"></a>启用成本访问

你是否看到消息“未经授权”，或“在注册中已禁用成本查看”。 在寻找成本信息？![未经授权](media/billing-enterprise-mgmt-groups/unauthorized.png)

这可能是以下原因之一造成的：

1. 通过企业合作伙伴购买的 Azure，而该合作伙伴尚未发布定价信息。 要发布定价，请与合作伙伴联系，以在[企业门户](https://ea.azure.com)中更新设置。
2. 此外，如果你是 EA Direct 客户，则有以下几种可能性：
    * 你是帐户所有者且你的注册管理员已禁用“AO 查看费用”设置。  
    * 你是部门管理员且你的注册管理员已禁用“DA 查看费用”设置。
    * 联系你的注册管理员以获取访问权限。 注册管理员可以访问[企业门户](https://ea.azure.com/manage/enrollment)并更新设置，如下所示：

![企业门户设置](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)


## <a name="asset-is-unavailable"></a>资产不可用？ 
如果在尝试访问订阅或管理组时收到错误消息“此资产不可用”，则不具有正确角色来查看此项目。  

![asset-not-found](media/billing-enterprise-mgmt-groups/asset-not-found.png)

联系订阅或管理组的管理员以获得访问权限。  
* 对于订阅，请参考 [Azure 基于角色的访问控制 (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) 文档，有助于了解需要哪个角色。
