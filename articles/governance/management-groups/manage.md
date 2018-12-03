---
title: 如何在 Azure 中更改、删除或管理管理组
description: 了解如何维护和更新管理组层次结构。
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 10dfa9812a0546f3a8c57e28227851b6f72657fc
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582405"
---
# <a name="manage-your-resources-with-management-groups"></a>使用管理组管理资源

管理组是一些容器，可以帮助跨多个订阅管理访问权限、策略和符合性。 可以更改、删除和管理这些容器来构建可与 [Azure Policy](../policy/overview.md) 和 [Azure 基于角色的访问控制 (RBAC)](../../role-based-access-control/overview.md) 配合使用的层次结构。 有关管理组的详细信息，请参阅[使用 Azure 管理组整理资源](overview.md)。

若要对管理组进行更改，必须在管理组中拥有“所有者”或“参与者”角色。 若要查看自己拥有哪些权限，请选择管理组，然后选择“IAM”。 有关 RBAC 角色的详细信息，请参阅[使用 RBAC 管理访问权限和权限](../../role-based-access-control/overview.md)。

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>更改管理组的名称

可以使用门户、PowerShell 或 Azure CLI 更改管理组的名称。

### <a name="change-the-name-in-the-portal"></a>在门户中更改名称

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 选择“所有服务” > “管理组”。

1. 选择要重命名的管理组。

1. 选择页面顶部的“重命名组”选项。

   ![重命名组](./media/detail_action_small.png)

1. 菜单打开后，请输入要显示的新名称。

   ![重命名组](./media/rename_context.png)

1. 选择“保存”。

### <a name="change-the-name-in-powershell"></a>在 PowerShell 中更改名称

若要更新显示名称，请使用 **Update-AzureRmManagementGroup**。 例如，若要将管理组的名称从“Contoso IT”更改为“Contoso Group”，可运行以下命令：

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>在 Azure CLI 中更改名称

在 Azure CLI 中使用 update 命令。

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>删除管理组

若要删除某个管理组，必须满足以下要求：

1. 该管理组下面没有任何子管理组或订阅。

   - 若要将订阅移出管理组，请参阅[将订阅移到另一个管理组](#Move-subscriptions-in-the-hierarchy)。

   - 若要将管理组移到另一个管理组，请参阅[在层次结构中移动管理组](#Move-management-groups-in-the-hierarchy)。

1. 在管理组中拥有管理组“所有者”或“参与者”角色的写入权限。 若要查看自己拥有哪些权限，请选择管理组，然后选择“IAM”。 有关 RBAC 角色的详细信息，请参阅[使用 RBAC 管理访问权限和权限](../../role-based-access-control/overview.md)。  

### <a name="delete-in-the-portal"></a>在门户中删除

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 选择“所有服务” > “管理组”。

1. 选择要删除的管理组。

1. 选择“删除”

   - 如果该图标已禁用，将鼠标指针悬停在该图标上可显示原因。

   ![删除组](./media/delete.png)

1. 此时会打开一个窗口，让你确认是否要删除该管理组。

   ![删除组](./media/delete_confirm.png)

1. 请选择“是”。

### <a name="delete-in-powershell"></a>在 PowerShell 中删除

在 PowerShell 中使用 **Remove-AzureRmManagementGroup** 命令删除管理组。

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="delete-in-azure-cli"></a>在 Azure CLI 中删除

在 Azure CLI 中，可以使用 az account management-group delete 命令。

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>查看管理组

可以查看你对其拥有直接管理角色或继承 RBAC 角色的任何管理组。  

### <a name="view-in-the-portal"></a>在门户中查看

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 选择“所有服务” > “管理组”。

1. 此时将加载“管理组”层次结构页，可以在其中浏览有权访问的所有管理组和订阅。 选择组名会将你带到层次结构中的下一级别。 导航的工作方式与文件资源管理器一样。

   ![主要](./media/main.png)

1. 若要查看管理组的详细信息，请选择管理组标题旁边的“(详细信息)”链接。 如果此链接不可用，则表示你无权查看该管理组。  

### <a name="view-in-powershell"></a>在 PowerShell 中查看

使用 Get-AzureRmManagementGroup 命令可检索所有组。  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```

若要查看单个管理组的信息，请使用 -GroupName 参数

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName 'Contoso'
```

### <a name="view-in-azure-cli"></a>在 Azure CLI 中查看

使用 list 命令可以检索所有组。  

```azurecli-interactive
az account management-group list
```

若要查看单个管理组的信息，请使用 show 命令

```azurecli-interactive
az account management-group show --name 'Contoso'
```

## <a name="move-subscriptions-in-the-hierarchy"></a>在层次结构中移动订阅

创建管理组的原因之一是将订阅捆绑在一起。 只能将管理组和订阅设置为另一个管理组的子级。 移到管理组的订阅从父管理组继承所有用户访问权限和策略。

若要移动订阅，必须拥有几项权限：

- 子订阅中的“所有者”角色。
- 新父管理组中的“所有者”或“参与者”角色。
- 旧父管理组中的“所有者”或“参与者”角色。

若要查看自己拥有哪些权限，请选择管理组，然后选择“IAM”。 有关 RBAC 角色的详细信息，请参阅[使用 RBAC 管理访问权限和权限](../../role-based-access-control/overview.md)。

### <a name="move-subscriptions-in-the-portal"></a>在门户中移动订阅

#### <a name="add-an-existing-subscription-to-a-management-group"></a>将现有订阅添加到管理组

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 选择“所有服务” > “管理组”。

1. 选择要设为父级的管理组。

1. 在页面顶部，选择“添加订阅”。

1. 在列表中选择具有正确 ID 的订阅。

   ![子级](./media/add_context_sub.png)

1. 选择“保存”。

#### <a name="remove-a-subscription-from-a-management-group"></a>从管理组删除订阅

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 选择“所有服务” > “管理组”。

1. 选择要设为当前父级的管理组。  

1. 在列表中，选择要移动的订阅所在行末尾的椭圆。

   ![移动](./media/move_small.png)

1. 选择“移动”。

1. 在打开的菜单中，选择“父管理组”。

   ![移动](./media/move_small_context.png)

1. 选择“保存”。

### <a name="move-subscriptions-in-powershell"></a>在 PowerShell 中移动订阅

若要在 PowerShell 中移动订阅，请使用 New-AzureRmManagementGroupSubscription 命令。  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

若要删除订阅与管理组之间的链接，请使用 Remove-AzureRmManagementGroupSubscription 命令。

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>在 Azure CLI 中移动订阅

若要在 CLI 中移动订阅，请使用 add 命令。

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

若要从管理组中删除订阅，请使用 subscription remove 命令。  

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

## <a name="move-management-groups-in-the-hierarchy"></a>在层次结构中移动管理组  

移动某个父管理组时，包含管理组、订阅、资源组和资源的所有子资源会连同父级一起移动。

### <a name="move-management-groups-in-the-portal"></a>在门户中移动管理组

1. 登录到 [Azure 门户](https://portal.azure.com)。

1. 选择“所有服务” > “管理组”。

1. 选择要设为父级的管理组。

1. 在页面顶部，选择“添加管理组”。

1. 在打开的菜单中，选择要使用新管理组或现有管理组。

   - 选择新管理组将创建一个新管理组。
   - 选择现有管理组将显示所有管理组的下拉列表，这些管理组可移动到此管理组。  

   ![移动](./media/add_context_MG.png)

1. 选择“保存”。

### <a name="move-management-groups-in-powershell"></a>在 PowerShell 中移动管理组

在 PowerShell 中使用 Update-AzureRmManagementGroup 命令将管理组移到不同的组下面。

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'Contoso' -ParentName 'ContosoIT'
```  

### <a name="move-management-groups-in-azure-cli"></a>在 Azure CLI 中移动管理组

在 Azure CLI 中使用 update 命令移动管理组。

```azurecli-interactive
az account management-group update --name 'Contoso' --parent 'Contoso Tenant'
```

## <a name="audit-management-groups-using-activity-logs"></a>使用活动日志审核管理组

若要通过此 API 跟踪管理组，请使用[租户活动日志 API](/rest/api/monitor/tenantactivitylogs)。 目前不可以使用 PowerShell、CLI 或 Azure 门户跟踪管理组活动。

1. Azure AD 租户的租户管理员可以[提升访问权限](../../role-based-access-control/elevate-access-global-admin.md)，然后将一个“读者”角色分配给 `/providers/microsoft.insights/eventtypes/management` 范围内的审核用户。
1. 以审核用户身份调用[租户活动日志 API](/rest/api/monitor/tenantactivitylogs) 来查看管理组活动。 需要按资源提供程序 **Microsoft.Management** 对所有管理组活动进行筛选。  示例：

```xml
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Management'"
```

> [!NOTE]
> 若要快速从命令行调用此 API，请尝试使用 [ARMClient](https://github.com/projectkudu/ARMClient)。

## <a name="next-steps"></a>后续步骤

若要了解有关管理组的详细信息，请参阅：

- [创建管理组来组织 Azure 资源](create.md)
- [如何更改、删除或管理管理组](manage.md)
- [在 Azure PowerShell 资源模块中查看管理组](https://aka.ms/mgPSdocs)
- [在 REST API 中查看管理组](https://aka.ms/mgAPIdocs)
- [在 Azure CLI 中查看管理组](https://aka.ms/mgclidoc)
