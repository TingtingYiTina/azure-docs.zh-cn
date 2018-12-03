---
title: 将 Azure API 管理服务部署到多个 Azure 区域 | Microsoft 文档
description: 了解如何将 Azure API 管理服务实例部署到多个 Azure 区域。
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: apimpm
ms.openlocfilehash: 27bfd3176ecad847f9bba2a62abd66b55484443b
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443009"
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>如何将 Azure API 管理服务实例部署到多个 Azure 区域

Azure API 管理多区域部署，该部署可使 API 发布者在任意数量的所需 Azure 区域中分配单个 Azure API 管理服务。 这有助于减少地理上分散的 API 使用者所感知的请求延迟，并且还改善其中一个区域处于离线状态时的服务可用性。

新的 Azure API 管理服务最初只在一个 Azure 区域（主要区域）中包含一个[单元][unit]。 可通过 Azure 门户轻松添加其他区域。 API 管理网关服务器部署到每个区域，并且调用流量将路由到最近的网关。 如果一个区域处于离线状态，则传入流量自动重定向到下一个最近的网关。

> [!NOTE]
> Azure API 管理仅复制跨区域的 API 网关组件。 服务管理组件仅托管在主要区域中。 如果主要区域发生服务中断，则无法向 Azure API 管理服务实例应用配置更改 - 包括设置或策略更新。

[!INCLUDE [premium.md](../../includes/api-management-availability-premium.md)]

## <a name="add-region"> </a>将 API 管理服务实例部署到新区域

> [!NOTE]
> 如果尚未创建 API 管理服务实例，请参阅[创建 API 管理服务实例][Create an API Management service instance]。

在 Azure 门户中，导航到 API 管理服务实例的“规模和定价”页。 

![“缩放”选项卡][api-management-scale-service]

若要部署到新的区域，请单击工具栏中的“+ 添加区域”。

![添加区域][api-management-add-region]

从下拉列表中选择位置，并通过滑块为其设置单位数。

![指定单位][api-management-select-location-units]

单击“添加”将选择放置在“位置”表中。 

重复此过程，直到配置所有位置，并单击工具栏中的“保存”，启动部署过程。

## <a name="remove-region"> </a>从位置中删除 API 管理服务实例

在 Azure 门户中，导航到 API 管理服务实例的“规模和定价”页。 

![“缩放”选项卡][api-management-scale-service]

若要删除位置，请使用表右端的“...”按钮打开上下文菜单。 选择“删除”选项。

确认删除，并单击“保存”应用所做的更改。

## <a name="route-backend"> </a>将 API 调用路由到区域后端服务

Azure API 管理只有一个后端服务 URL。 即使不同的区域中存在 Azure API 管理实例，API 网关也仍会将请求转发到只在一个区域中部署的同一后端服务。 在这种情况下，只有特定于该请求的区域中 Azure API 管理缓存的响应才能提升性能，但访问全球范围的后端时仍可能导致较高的延迟。

若要充分利用系统的地理分布性，应在 Azure API 管理实例所在的同一区域中部署后端服务。 然后，可以使用策略和 `@(context.Deployment.Region)` 属性将流量路由到后端的本地实例。

1. 导航到 Azure API 管理实例，然后在左侧菜单中单击“API”。
2. 选择所需的 API。
3. 在“入站处理”中的箭头式下拉列表内单击“代码编辑器”。

    ![API 代码编辑器](./media/api-management-howto-deploy-multi-region/api-management-api-code-editor.png)

4. 结合使用 `set-backend` 和 `choose` 条件策略，在文件的 `<inbound> </inbound>` 节中构建适当的路由策略。

    例如，以下 XML 文件适用于美国西部和东亚区域：

    ```xml
    <policies>
        <inbound>
            <base />
            <choose>
                <when condition="@("West US".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-us.com/" />
                </when>
                <when condition="@("East Asia".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-asia.com/" />
                </when>
                <otherwise>
                    <set-backend-service base-url="http://contoso-other.com/" />
                </otherwise>
            </choose>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>
    ```

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: get-started-create-service-instance.md
[Get started with Azure API Management]: get-started-create-service-instance.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/
