---
title: 预付 Azure Cosmos DB 资源费用以节省资金 | Microsoft Docs
description: 了解如何购买 Azure Cosmos DB 预留容量以节省计算成本。
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: d389b3b7e50c83246542fd6b446827055a5a002a
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52583460"
---
# <a name="prepay-for-azure-cosmos-db-resources-with-reserved-capacity"></a>通过预留容量预付 Azure Cosmos DB 资源费用

Azure Cosmos DB 预留容量有助于通过预付为期一年或三年的 Azure Cosmos DB 资源费用为你省钱。 使用 Azure Cosmos DB 预留容量，可以获得为 Cosmos DB 资源预配的吞吐量的折扣。 资源示例包括数据库和容器（表、集合和图）。

Azure Cosmos DB 预留容量可以大幅降低 Cosmos DB 成本，相比正常价格最多可以降低 65%，前提是提前承诺使用一年或三年的服务。 预留容量提供一种计费折扣，并且不会影响 Azure Cosmos DB 资源的运行时状态。

Azure Cosmos DB 预留容量涵盖为资源预配的吞吐量的费用。 但它不涵盖存储和网络费用。 购买预留后，不再像即用即付资费一样收取与预留属性相应的吞吐量费用。 预留相关详细信息，请参阅 [Azure 预留](../billing/billing-save-compute-costs-reservations.md)一文。 

可从 [Azure 门户](https://portal.azure.com)购买 Azure Cosmos DB 预留容量。 购买预留容量：

* 必须至少是一个企业或即用即付订阅的“所有者”角色。  
* 对于企业订阅，必须在 [EA 门户](https://ea.azure.com/)中启用 Azure 预留容量购买。  
* 对于云解决方案提供商 (CSP) 计划，只有管理员代理或销售代理可以购买 Azure Cosmos DB 预留容量。

## <a name="determine-the-required-throughput-before-purchase"></a>在购买之前确定所需吞吐量

预留大小基于现有的或即将部署的 Azure Cosmos DB 资源将使用的吞吐量总量。 可按以下方式确定所需的吞吐量：

* 获取跨所有区域的 Azure Cosmos DB 帐户、数据库和集合中的总预配吞吐量的历史数据。 例如，可以通过从 `https://account.azure.com` 下载每日使用情况报表来评估每日平均预配吞吐量。

* 如果你是企业协议 (EA) 客户，可以下载使用情况文件来获取 Azure Cosmos DB 吞吐量详细信息。 请参阅使用情况文件的“其他信息”部分中的“服务类型”值。

* 可以将 Azure Cosmos DB 帐户中预计在接下来的一年或三年中运行的所有工作负载的平均吞吐量相加。 然后可以将相加后的数量用作预留。

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>购买 Azure Cosmos DB 预留容量

1. 登录到 [Azure 门户](https://portal.azure.com)。  

2. 选择“所有服务” > “预留” > “添加”。  

3. 从“选择产品类型”窗格中依次选择“Azure Cosmos DB” > “选择”，以购买新预留。  

4. 填写必填字段，如下表所示：

   ![填写预留容量表](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png) 

   |字段  |Description  |
   |---------|---------|
   |名称   |    预留的名称。 该字段自动填充 `CosmosDB_Reservation_<timeStamp>`。 创建预留时，可以提供不同的名称。 或者，可以在创建预留后对其重命名。      |
   |订阅  |   用于支付 Azure Cosmos DB 预留容量费用的订阅。 将向所选订阅的付款方式收取预付费用。 订阅类型必须是以下类型之一： <br/><br/>  [企业协议](https://azure.microsoft.com/pricing/enterprise-agreement/)（套餐编号：MS-AZR-0017P）：对于企业订阅，从注册的货币承诺余额中扣除费用或作为超额收取费用。 <br/><br/> [即用即付](https://azure.microsoft.com/offers/ms-azr-0003p/)（套餐编号：MS-AZR-0003P）：对于即用即付订阅，将向订阅的信用卡或发票付款方式收取费用。    |
   |范围   |   控制有多少订阅可以使用与预留关联的计费权益的选项。 它还控制将预留应用于特定订阅的方式。   <br/><br/>  如果选择“单个订阅”，预留折扣将应用到所选订阅中的 Azure Cosmos DB 实例。 <br/><br/>  如果选择“共享”，预留折扣将应用到计费上下文内任何订阅中运行的 Azure Cosmos DB 实例。 计费上下文基于 Azure 的注册方式。 对于企业客户，共享范围是许可登记表，包括许可登记表中的所有订阅（开发/测试订阅除外）。 对于即用即付客户，共享范围是由帐户管理员创建的所有即用即付订阅。  <br/><br/> 购买预留容量后，可以更改预留范围。  |
   |预留容量类型   |  预配为请求单位的吞吐量。|
   |预留容量单位  |      想预留的吞吐量的量。 可以通过确定每个区域的所有 Cosmos DB 资源（例如，数据库或容器）所需的吞吐量来计算此值。 然后，将它乘以将与 Cosmos DB 数据库关联的区域数。  <br/><br/> 例如：如果有五个区域，每个区域 1 百万 RU/秒，则购买预留容量时应选 5 百万 RU/秒。    |
   |术语  |   一年或三年。   |

5. 在“费用”部分查看预留的折扣或价格。 此预留价格适用于使用跨所有区域预配的吞吐量的 Azure Cosmos DB 资源。  

6. 选择“购买”。 购买成功后，会看到如下页面所示内容： 

   ![填写预留容量表](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png) 

购买预留后，它立即应用到与预留期相符的所有现有 Azure Cosmos DB 资源。 如果还没有 Azure Cosmos DB 资源，则将在你部署符合预留期的新 Cosmos DB 实例时应用预留。 这两种情况的预留期均于成功购买后立即开始。 

预留到期时，Azure Cosmos DB 实例会继续运行，按正常即用即付资费计费。

## <a name="next-steps"></a>后续步骤

预留折扣自动应用于与预留范围和属性相符的 Azure Cosmos DB 资源。 可以通过 Azure 门户、PowerShell、Azure CLI 或 API 更新预留的范围。

*  要了解预留容量折扣是如何应用到 Azure Cosmos DB 的，请参阅[了解 Azure 预留折扣](../billing/billing-understand-cosmosdb-reservation-charges.md)。

* 若要了解有关 Azure 预留的详细信息，请参阅以下文章：

   * [什么是 Azure 预留？](../billing/billing-save-compute-costs-reservations.md)  
   * [管理 Azure 预留](../billing/billing-manage-reserved-vm-instance.md)  
   * [了解企业合约的预留使用情况](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [了解即用即付订阅的预留使用情况](../billing/billing-understand-reserved-instance-usage.md)
   * [合作伙伴中心 CSP 计划中的 Azure 预留](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>需要帮助？ 请联系我们。

如果你有任何疑问或需要帮助，请[创建支持请求](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)。

