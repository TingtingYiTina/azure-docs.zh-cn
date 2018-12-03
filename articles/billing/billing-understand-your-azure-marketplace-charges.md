---
title: 了解 Azure 外部服务收费 | Microsoft Docs
description: 了解 Azure 中外部服务（以前称为市场）的计费。
services: ''
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.assetid: 5e0e2a3c-d111-4054-8508-0c111c1b749b
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: cwatson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0e90c043976fc1798f675f8bf480b02e0ed93536
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581912"
---
# <a name="understand-your-azure-billing-for-external-service-charges"></a>了解外部服务收费的 Azure 计费
外部服务由 Azure 市场中的第三方软件供应商发布。 例如，SendGrid 是可以在 Azure 中购买的外部服务，但不是 Microsoft 发布的。

预配新的外部服务或资源时，会显示警告：

![市场购买警告](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

> [!NOTE]
> 外部服务是 Microsoft 以外的公司发布的，但有时候，Microsoft 产品也会被归类为外部服务。
> 
> 

## <a name="how-external-services-are-billed"></a>外部服务的计费方式
- 外部服务单独计费。 它们被视为 Azure 订阅中的单个订单。 购买每个服务时，会设置该服务的计费周期。 请勿将其与购买的服务所属的订阅的计费周期混淆。 用户会收到单独的帐单，信用卡也会单独收费。
- 每个外部服务都有不同的计费模型。 某些服务按即用即付方式计费，某些服务则使用按月付款模式。 需要使用信用卡购买 Azure 外部服务，不能使用发票付款方式购买外部服务。
- 购买外部服务时不能使用每月的免费信用额度。 如果所使用的 Azure 订阅包含[免费信用额度](https://azure.microsoft.com/pricing/spending-limits/)，则不能将其用于外部服务帐单。 使用信用卡购买外部服务。

## <a name="view-external-service-spending-and-history-in-the-azure-portal"></a>在 Azure 门户中查看外部服务支出和历史记录
可以在 [Azure 门户](https://portal.azure.com/)中查看每个订阅的外部服务的列表： 

1. 以帐户管理员身份登录到 [Azure 门户](https://portal.azure.com/)。
2. 在“中心”菜单中，选择“订阅”。
   
    ![在“中心”菜单中，选择“订阅”](./media/billing-understand-your-azure-marketplace-charges/sub-button.png) 
3. 在“订阅”边栏选项卡中，选择要查看的订阅，并选择“外部服务”。
   
    ![在计费边栏选项卡中选择订阅](./media/billing-understand-your-azure-marketplace-charges/select-sub-external-services.png)
4. 此时会看到每个外部服务订单、发布者名称、购买的服务层、提供给资源的名称，以及当前订单状态。 若要查看过去的帐单，请选择一项外部服务。
   
    ![选择外部服务](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)
5. 在此处可查看过去的帐单金额，包括税款明细。
   
    ![查看外部服务帐单](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="view-external-service-spending-for-enterprise-agreement-ea-customers"></a>查看针对企业协议 (EA) 客户的外部服务支出
EA 客户可以在 EA 门户中查看外部服务支出和下载报表。 若要开始使用，请参阅[适用于 EA 客户的 Azure 市场](https://ea.azure.com/helpdocs/azureMarketplace)。

## <a name="manage-payment-methods-for-external-service-orders"></a>管理外部服务订单的付款方式
在[帐户中心](https://account.windowsazure.com/)更新外部服务订单的付款方式。

> [!NOTE]
> 如果是使用工作帐户或学校帐户购买的订阅，请[联系支持人员](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)以对付款方式进行更改。
> 
> 

1. 登录到[帐户中心](https://account.windowsazure.com/)，[导航到“市场”选项卡](https://account.windowsazure.com/Store)
   
    ![在帐户中心选择“市场”](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)
2. 选择要管理的外部服务
   
    ![选择要管理的外部服务](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)
3. 在页面右侧，单击“更改付款方式”。 此链接指向另一管理付款方式的门户。
   
    ![订单摘要](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)
4. 单击“编辑信息”，按说明更新付款信息。
   
    ![选择“编辑信息”](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)

## <a name="cancel-an-external-service-order"></a>取消外部服务订单
若要取消外部服务订单，请在 [Azure 门户](https://portal.azure.com)中删除资源。

![删除资源](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-us"></a>需要帮助？ 请联系我们。

如果你有任何疑问或需要帮助，请[创建支持请求](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)。

