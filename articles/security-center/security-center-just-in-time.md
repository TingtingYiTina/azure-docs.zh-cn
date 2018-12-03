---
title: Azure 安全中心中的恰时虚拟机访问 | Microsoft Docs
description: 本文档说明 Azure 安全中心中的恰时 VM 访问可以如何帮助你控制对 Azure 虚拟机的访问。
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: rkarlin
ms.openlocfilehash: 72acf0f06bbed0129ff322b10a7faf16fd94f712
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314739"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>使用恰时功能管理虚拟机访问

恰时虚拟机 (VM) 访问可以用来锁定发往 Azure VM 的入站流量，降低遭受攻击的可能性，同时在需要时还允许轻松连接到 VM。

> [!NOTE]
> 实时功能在安全中心的标准层上可用。  若要详细了解安全中心的定价层，请参阅[定价](security-center-pricing.md)。
>
>

## <a name="attack-scenario"></a>攻击方案

暴力攻击通常以攻击管理端口为手段来获取对 VM 的访问权限。 如果成功，则攻击者可以获得对 VM 的控制权并建立通向你的环境的据点。

降低遭受暴力攻击的可能性的一种方法是限制端口处于打开状态的时间量。 管理端口不需要始终处于打开状态。 它们只需要在特定的时间打开，例如在你连接到 VM 来执行管理或维护任务时。 当启用了恰时功能时，安全中心会使用[网络安全组](../virtual-network/security-overview.md#security-rules) (NSG) 规则，这些规则将限制对管理端口的访问以使其不会成为攻击者的目标。

![恰时方案][1]

## <a name="how-does-just-in-time-access-work"></a>恰时访问的工作原理是怎样的？

当启用了恰时访问时，安全中心会通过创建 NSG 规则来锁定发往 Azure VM 的入站流量。 你需要选择要锁定 VM 上的哪些端口的入站流量。 这些端口将受恰时解决方案控制。

当用户请求访问 VM 时，安全中心会检查该用户是否具有为 VM 提供写入访问的[基于角色的访问控制 (RBAC)](../role-based-access-control/role-assignments-portal.md) 权限。 如果用户具有写入权限，则会批准请求并且安全中心会自动将网络安全组 (NSG) 配置为：在你指定的时间内允许发往选定端口的入站流量。 在该时间到期后，安全中心会将 NSG 还原为以前的状态。 但是，那些已经建立的连接不会中断。

> [!NOTE]
> 安全中心恰时 VM 访问当前仅支持通过 Azure 资源管理器部署的 VM。 若要了解有关经典部署模型和资源管理器部署模型的详细信息，请参阅 [Azure 资源管理器与经典部署](../azure-resource-manager/resource-manager-deployment-model.md)。
>
>

## <a name="using-just-in-time-access"></a>使用恰时访问

1. 打开“安全中心”仪表板。

2. 在左侧窗格中，选择“恰时 VM 访问”。

![“恰时 VM 访问”磁贴][2]

“恰时 VM 访问”窗口随即打开。

![“恰时 VM 访问”磁贴][10]

“恰时 VM 访问”提供 VM 的状态信息：

- **已配置** - 已配置为支持恰时 VM 访问的 VM。 提供的数据是针对过去一周的，并且针对每个 VM 包括了已批准的请求数、上次访问日期和时间以及上一个用户。
- **推荐** - 可以支持恰时 VM 访问但尚未配置此功能的 VM。 建议为这些 VM 启用恰时 VM 访问控制。 请参阅[配置恰时访问策略](#configuring-a-just-in-time-access-policy)。
- **不推荐** - 导致不推荐某个 VM 的可能原因有：
  - 缺少 NSG - 恰时解决方案需要 NSG 准备就绪。
  - 经典 VM - 安全中心恰时 VM 访问当前仅支持通过 Azure 资源管理器部署的 VM。 恰时解决方案不支持经典部署。
  - 其他 - 如果在订阅或资源组的安全策略中未开启恰时解决方案，或者 VM 缺少公共 IP 且没有已准备就绪的 NSG，则该 VM 将位于此类别中。

## <a name="configuring-a-just-in-time-access-policy"></a>配置恰时访问策略

选择要启用的 VM：

1. 在“恰时 VM 访问”下，选择“推荐”选项卡。

  ![启用恰时访问][3]

2. 在“虚拟机”下，选择要启用的 VM。 这会在 VM 旁边放置一个复选标记。
3. 选择“在 VM 上启用 JIT”。
4. 选择“保存”。

### <a name="default-ports"></a>默认端口

可以查看安全中心推荐为其启用恰时功能的默认端口。

1. 在“恰时 VM 访问”下，选择“推荐”选项卡。

  ![显示默认端口][6]

2. 在“VM”下，选择一个 VM。 这会在该 VM 旁边放置一个复选标记并打开“JIT VM 访问配置”。 此边栏选项卡将显示默认端口。

### <a name="add-ports"></a>添加端口

在“JIT VM 访问配置”下，还可以添加并配置要启用恰时解决方案的新端口。

1. 在“JIT VM 访问配置”下，选择“添加”。 这将打开“添加端口配置”。

  ![端口配置][7]

2. 在“添加端口配置”下，标识端口、协议类型、允许的源 IP 和最大请求时间。

  允许的源 IP 是允许根据批准的请求获取访问权限的 IP 范围。

  最大请求时间是特定端口可以处于打开状态的最大时间范围。

3. 选择“确定”。

> [!NOTE]
>如果为 VM 启用 JIT VM 访问，Azure 安全中心将在与所选端口关联的网络安全组中为该端口创建拒绝所有入站通信规则。 规则在网络安全组中将具有最高优先级，或比其中的现有规则的优先级更低。 这取决于确定规则是否安全的 Azure 安全中心执行的分析。
>


## <a name="set-just-in-time-within-a-vm"></a>在 VM 中设置实时访问

若要轻松地在 VM 间进行实时访问，可以将 VM 设置为仅允许从 VM 内直接进行实时访问。

1. 在 Azure 门户中，选择“虚拟机”。
2. 单击要实行实时访问限制的虚拟机。
3. 在菜单中，单击“配置”。
4. 在“实时访问”下，单击“启用实时策略”。 

此操作可为使用以下设置的 VM 启用实时访问：

- Windows 服务器：
    - RDP 端口 3389
    - 3 小时访问
    - 允许的源 IP 地址设置为“按请求”
- Linux 服务器：
    - SSH 端口 22
    - 3 小时访问
    - 允许的源 IP 地址设置为“按请求”
     
如果 VM 已启用实时访问，则在转到其配置页时，将看到实时访问已启用。可使用链接打开 Azure 安全中心中的策略，以便查看和更改设置。

![VM 中的 JIT 配置](./media/security-center-just-in-time/jit-vm-config.png)


## <a name="requesting-access-to-a-vm"></a>请求对 VM 的访问权限

若要请求对 VM 的访问权限，请执行以下操作：

1. 在“恰时 VM 访问”下，选择“配置”选项卡。
2. 在“VM”下，选择要启用访问权限的 VM。 这会在 VM 旁边放置一个复选标记。
3. 选择“请求访问权限”。 这将打开“请求访问权限”。

  ![请求对 VM 的访问权限][4]

4. 在“请求访问权限”下，为每个 VM 配置要打开的端口、要为其打开该端口的源 IP 以及将打开该端口的时间范围。 只能请求对在恰时策略中配置的端口的访问权限。 每个端口都有一个从恰时策略派生的最大允许时间。
5. 选择“打开端口”。

> [!NOTE]
> 当用户请求访问 VM 时，安全中心会检查该用户是否具有为 VM 提供写入访问的[基于角色的访问控制 (RBAC)](../role-based-access-control/role-assignments-portal.md) 权限。 如果用户具有写入权限，则会批准请求。
>
>

> [!NOTE]
> 如果请求访问的用户使用代理，则“我的 IP”选项可能无法使用。 可能需要定义组织的完整范围。
>
>

## <a name="editing-a-just-in-time-access-policy"></a>编辑恰时访问策略

可以对 VM 的现有恰时策略进行以下更改：添加并配置要为该 VM 打开的新端口，或更改与已保护的端口相关的任何其他参数。

若要编辑 VM 的现有恰时策略，需使用“推荐”选项卡：

1. 在“VM”下，通过单击 VM 对应的行内的三个点来选择要为其添加端口的 VM。 这将打开一个菜单。
2. 在菜单中选择“编辑”。 这将打开“JIT VM 访问配置”。

  ![编辑策略][8]

3. 在“JIT VM 访问配置”下，可以通过单击某个已保护的端口来编辑该端口的现有设置，也可以选择“添加”。 这将打开“添加端口配置”。

  ![添加端口][7]

4. 在“添加端口配置”下，标识端口、协议类型、允许的源 IP 和最大请求时间。
5. 选择“确定”。
6. 选择“保存”。

## <a name="auditing-just-in-time-access-activity"></a>审核恰时访问活动

可以使用日志搜索深入了解 VM 活动。 若要查看日志，请执行以下操作：

1. 在“恰时 VM 访问”下，选择“配置”选项卡。
2. 在“VM”下，通过单击 VM 对应的行内的三个点来选择要查看其信息的 VM。 这将打开一个菜单。
3. 在菜单中选择“活动日志”。 这将打开“活动日志”。

  ![选择“活动日志”][9]

  “活动日志”提供了该 VM 的以前操作的经筛选视图以及时间、日期和订阅。

可以通过选择“单击此处将所有项下载为 CSV”来下载日志信息。

修改筛选器并选择“应用”来创建搜索和日志。

## <a name="using-just-in-time-vm-access-via-rest-apis"></a>通过 REST API 使用及时 VM 访问

通过 Azure 安全中心 API 可以使用及时 VM 访问功能。 可以通过此 API 获取有关配置 VM 的信息、添加新的 VM、请求访问 VM 等。 若要详细了解及时 REST API，请参阅 [Jit Network Access Policies](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies)（Jit 网络访问策略）。

## <a name="using-just-in-time-vm-access-via-powershell"></a>通过 PowerShell 使用恰时 VM 访问 

若要通过 PowerShell 使用实时 VM 访问解决方案，请使用正式的 Azure 安全中心 PowerShell cmdlet，具体为 `Set-AzureRmJitNetworkAccessPolicy`。

下面的示例可对特定 VM 设置实时 VM 访问策略，并设置以下各项：
1.  关闭端口 22 和 3389。
2.  将每个的最大时间窗口设置为 3 小时，使它们能够按每个批准的请求打开。
3.  允许请求访问的用户控制源 IP 地址，允许用户对批准的实时访问请求建立成功会话。

在 PowerShell 中运行以下命令实现此目的：

1.  分配变量，保存 VM 的实时 VM 访问策略:

        $JitPolicy = (@{
         id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
             number=22;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"},
             @{
             number=3389;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"})})

2.  将 VM 实时 VM 访问策略插入数组：
    
        $JitPolicyArr=@($JitPolicy)

3.  对所选 VM 配置实时 VM 访问策略：
    
        Set-AzureRmJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

### <a name="requesting-access-to-a-vm"></a>请求对 VM 的访问权限

在以下示例中，可以看到对特定 VM 的实时 VM 访问请求，其中请求端口 22 为特定 IP 地址打开，并持续特定时间：

在 PowerShell 中运行以下命令：
1.  配置 VM 请求访问属性

        $JitPolicyVm1 = (@{
          id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
           number=22;
           endTimeUtc="2018-09-17T17:00:00.3658798Z";
           allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.  在数组中插入 VM 访问请求参数：

        $JitPolicyArr=@($JitPolicyVm1)
3.  发送请求访问权限（使用步骤 1 中获取的资源 ID）

        Start-AzureRmJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

有关详细信息，请参阅 PowerShell cmdlet 文档。


## <a name="next-steps"></a>后续步骤
在本文中，你已了解了安全中心中的恰时 VM 访问如何帮助你控制对 Azure 虚拟机的访问。

若要了解有关安全中心的详细信息，请参阅以下文章：

- [设置安全策略](security-center-azure-policy.md) — 了解如何为 Azure 订阅和资源组配置安全策略。
- [管理安全建议](security-center-recommendations.md) — 了解建议如何帮助你保护 Azure 资源。
- [安全运行状况监视](security-center-monitoring.md) — 了解如何监视 Azure 资源的运行状况。
- [管理和响应安全警报](security-center-managing-and-responding-alerts.md) — 了解如何管理和响应安全警报。
- [监视合作伙伴解决方案](security-center-partner-solutions.md) — 了解如何监视合作伙伴解决方案的运行状态。
- [安全中心常见问题](security-center-faq.md) - 查找有关如何使用服务的常见问题。
- [Azure 安全性博客](https://blogs.msdn.microsoft.com/azuresecurity/) - 查找关于 Azure 安全性及合规性的博客文章。


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[10]: ./media/security-center-just-in-time/just-in-time-access.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
