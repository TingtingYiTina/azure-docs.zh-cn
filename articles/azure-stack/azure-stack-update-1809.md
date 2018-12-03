---
title: Azure Stack 1809 更新 |Microsoft Docs
description: 了解如何针对 Azure Stack 集成系统，包括已知的问题的新变化 1809年更新和下载更新的位置。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 9afce9c6d4ed4d6dc6fbe5bcfcfedc33bdd7cfdf
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314658"
---
# <a name="azure-stack-1809-update"></a>Azure Stack 1809 更新

*适用于：Azure Stack 集成系统*

本文介绍 1809年更新包的内容。 此更新包包含此版 Azure Stack 的改进、修复和已知问题。 本文还包含一个用于下载更新的链接。 已知问题分为与更新过程直接相关的问题，以及内部版本（安装后）的问题。

> [!IMPORTANT]  
> 此更新包仅适用于 Azure Stack 集成系统。 请勿将此更新包应用于 Azure Stack 开发工具包。

## <a name="build-reference"></a>内部版本参考

Azure Stack 1809 更新内部版本号是**1.1809.0.90**。  

### <a name="new-features"></a>新增功能

此更新包括适用于 Azure Stack 的以下改进：

- 此版本中，Azure Stack 集成系统的 4-16 节点的支持配置。 可以使用[Azure Stack 容量规划器](https://aka.ms/azstackcapacityplanner)以帮助在规划过程中以 Azure Stack 功能和配置。

- <!--  2712869   | IS  ASDK -->  **Azure Stack syslog 客户端 （公开上市）** 此客户端允许转发的审核、 警报和到 syslog 服务器或安全信息和事件管理 (SIEM) 软件的 Azure Stack 基础结构相关的安全日志Azure Stack 的外部。 syslog 客户端现在支持指定 syslog 服务器侦听的端口。

   此版本意味着 syslog 客户端已正式发布，可以用于生产环境。

   有关详细信息，请参阅 [Azure Stack Syslog 转发](azure-stack-integrate-security.md)。

- 现在可以[移动的注册资源](azure-stack-registration.md#move-a-registration-resource)而无需重新注册的资源组之间的 Azure 上。 云解决方案提供商 (Csp) 可以还注册资源在订阅之间移动，只要两个新的和旧订阅映射到相同的 CSP 合作伙伴 id。 这不会影响现有的客户租户映射。 

### <a name="fixed-issues"></a>修复的问题

<!-- TBD - IS ASDK -->
- 在门户中，报告免费/所用容量内存图表现在是精确的。 现在，您可以更可靠地预测可以创建多少个 Vm。

<!-- TBD - IS ASDK --> 
- 修复了在其中你在 Azure Stack 用户门户中，创建虚拟机，并在门户显示可以将附加到 DS 系列 VM 的数据磁盘数目不正确的问题。 DS 系列 VM 可以容纳的数据磁盘数取决于 Azure 配置。

- 以下的托管的磁盘问题固定的中 1809，而且还修复中 1808年[Azure Stack 修补程序 1.1808.5.110](https://support.microsoft.com/help/4468920/): 

   <!--  2966665 – IS, ASDK --> 
   - 在为高级托管的磁盘的虚拟机 （DS、 DSv2、 Fs、 Fs_V2） 失败，出现错误的大小的附加 SSD 数据磁盘中修复的问题：*未能更新虚拟机 vmname 错误的磁盘： 无法执行操作，因为请求的存储帐户类型 Premium_LRS 不支持的 VM 大小 Standard_DS/Ds_V2/FS/Fs_v2)*。 
   
   - 通过使用创建 VM 的托管的磁盘**createOption**:**附加**失败，出现以下错误：*长时间运行的操作失败，状态为失败。其他信息: 发生了内部执行错误。*
   错误代码： InternalExecutionError ErrorMessage： 内部执行时出错。
   
   现已修复此问题。

- <!-- 2702741 -  IS, ASDK --> 修复了在发出“停止-解除分配”命令后，无法保证系统会保留使用动态分配方法部署的公共 IP 的问题。 它们现在已保留。

- <!-- 3078022 - IS, ASDK --> 如果 VM 已停止解除分配，1808年之前它不能重新分配 1808年更新后。  此问题已在 1809 中解决。 处于这种状态且无法启动的实例可以在已应用此修复的 1809 中启动。 此修复还可以防止该问题反复发生。

<!-- 3090289 – IS, ASDK --> 
- 已修复问题，其中应用 1808年更新后，你可能会遇到以下问题部署包含托管磁盘的 Vm 时：

   1. 如果订阅是在 1808 更新之前创建的，通过托管磁盘部署 VM 可能会失败并出现内部错误消息。 若要解决此错误，请针对每个订阅执行以下步骤：
      1. 在租户门户中转到“订阅”，找到相应订阅。 依次单击“资源提供程序”、“Microsoft.Compute”、“重新注册”。
      2. 在同一订阅下，转到“访问控制(标识和访问管理)”，验证“Azure Stack - 托管磁盘”是否已列出。
   2. 如果已配置多租户环境，在与来宾目录相关联的订阅中部署 VM 可能会失败并出现内部错误消息。 若要解决该错误，请执行以下步骤：
      1. 应用 [1808 Azure Stack 修补程序](https://support.microsoft.com/help/4471992)。
      2. 执行[此文](azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory)中的步骤，重新配置每个来宾目录。


### <a name="changes"></a>更改

<!-- 2635202 - IS, ASDK -->
- 基础结构备份服务将从移[公钥基础结构网络](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-network#public-infrastructure-network)到[公共 VIP 网络](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-network#public-vip-network)。 客户将需要确保该服务具有公共 VIP 网络中访问备份存储位置。  

> [!IMPORTANT]  
> 如果您不允许公共 VIP 网络中的连接到文件服务器的防火墙，此更改将导致基础结构备份失败，"错误 53 找不到的网络路径。" 这是一项重大更改具有合理的解决方法。 根据客户反馈，Microsoft 将恢复此修补程序中的更改。 请查看[发布更新步骤一节](#post-update-steps)有关 1809年上可用的修补程序详细信息。 可用修补程序后，请确保将其应用更新到 1809年，仅当您的网络策略不允许访问基础结构资源的公共 VIP 网络后。 在 1811，此更改将应用于所有系统。 如果在 1809年应用修补程序，则无需任何进一步操作。  

### <a name="common-vulnerabilities-and-exposures"></a>常见漏洞和风险

此更新将安装以下安全更新：  

- [ADV180022](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180022)
- [CVE 2018 0965](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0965)
- [CVE 2018 8271](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8271)
- [CVE 2018 8320](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8320)
- [CVE 2018 8330](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8330)
- [CVE 2018 8332](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8332)
- [CVE 2018 8333](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8333)
- [CVE 2018 8335](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8335)
- [CVE 2018 8392](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8392)
- [CVE 2018 8393](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8393)
- [CVE 2018 8410](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8410)
- [CVE 2018 8411](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8411)
- [CVE 2018 8413](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8413)
- [CVE 2018 8419](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8419)
- [CVE 2018 8420](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8420)
- [CVE 2018 8423](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8423)
- [CVE 2018 8424](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8424)
- [CVE 2018 8433](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8433)
- [CVE 2018 8434](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8434)
- [CVE 2018 8435](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8435)
- [CVE 2018 8438](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8438)
- [CVE 2018 8439](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8439)
- [CVE 2018 8440](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8440)
- [CVE 2018 8442](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8442)
- [CVE 2018 8443](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8443)
- [CVE 2018 8446](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8446)
- [CVE 2018 8449](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8449)
- [CVE 2018 8453](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8453)
- [CVE 2018 8455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8455)
- [CVE 2018 8462](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8462)
- [CVE 2018 8468](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8468)
- [CVE 2018 8472](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8472)
- [CVE 2018 8475](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8475)
- [CVE 2018 8481](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8481)
- [CVE 2018 8482](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8482)
- [CVE 2018 8484](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8484)
- [CVE 2018 8486](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8486)
- [CVE 2018 8489](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8489)
- [CVE 2018 8490](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8490)
- [CVE 2018 8492](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8492)
- [CVE 2018 8493](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8493)
- [CVE 2018 8494](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8494)
- [CVE 2018 8495](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8495)
- [CVE 2018 8497](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8497)

有关这些漏洞的详细信息，单击上面的链接，或请参阅 Microsoft 知识库文章[4457131](https://support.microsoft.com/help/4457131)并[4462917](https://support.microsoft.com/help/4462917)。

### <a name="prerequisites"></a>必备组件

- 为 1808年应用 1809年之前安装最新的 Azure Stack 修补程序。 有关详细信息，请参阅[KB 4471992 – Azure Stack 修补程序 Azure Stack 修补程序 1.1808.7.113](https://support.microsoft.com/help/4471992/)。

  > [!TIP]  
  > 订阅下述 *RRS* 或 *Atom* 源，了解 Azure Stack 修补程序的最新更新：
  > - RRS： https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss … 
  > - Atom： https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom …


- 在开始安装此更新之前，请使用以下参数运行 [Test-AzureStack](azure-stack-diagnostic-test.md)，以验证 Azure Stack 的状态并解决发现的所有操作问题，包括所有警告和故障。 另外，请查看活动警报，并解决所有需要采取措施的警报。  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

### <a name="known-issues-with-the-update-process"></a>更新过程的已知问题

- 在运行时[Test-azurestack](azure-stack-diagnostic-test.md) 1809年更新后，将显示警告消息从基板管理控制器 (BMC)。 可以放心地忽略此警告。

- <!-- 2468613 - IS --> 在安装此更新期间，可能会出现标题如下的警报：“错误 - 缺少 FaultType UserAccounts.New 的模板”。  可以放心地忽略这些警报。 完成此更新的安装后，这些警报会自动关闭。

- <!-- 2489559 - IS --> 在安装此更新的过程中，请勿尝试创建虚拟机。 有关如何管理更新的详细信息，请参阅[在 Azure Stack 中管理更新的概述](azure-stack-updates.md#plan-for-updates)。

- <!-- 3139614 | IS --> 如果已从您的 OEM，对 Azure Stack 应用更新**可用更新**通知可能不会显示在 Azure Stack 管理门户中。 若要安装 Microsoft 更新，请下载并导入它使用位于此处的说明手动[在 Azure Stack 中应用更新](azure-stack-apply-updates.md)。

### <a name="post-update-steps"></a>更新后步骤

> [!Important]  
> 获取 Azure Stack 部署准备好进行扩展主机启用的下一个更新包。 使用以下指南对系统进行准备[准备适用于 Azure Stack 扩展主机](azure-stack-extension-host-prepare.md)。

安装此更新之后，请安装所有适用的修补程序。 有关详细信息，请查看以下知识库文章，以及我们的[服务策略](azure-stack-servicing-policy.md)。  
- [KB 4477849 – Azure Stack 修补程序 Azure Stack 修补程序 1.1809.6.102](https://support.microsoft.com/help/4477849/)  

## <a name="known-issues-post-installation"></a>已知问题（安装后）

下面是此内部版本的安装后已知问题。

### <a name="portal"></a>门户

- Azure Stack 技术文档重点介绍了最新版本。 由于版本之间的门户更改，在使用 Azure Stack 门户时看到的内容可能与在文档中看到的内容不同。

<!-- 2930718 - IS ASDK --> 
- 在管理员门户中访问用户订阅详细信息时，在关闭边栏选项卡并单击“最新”以后，用户订阅名称不显示。

<!-- 3060156 - IS ASDK --> 
- 在管理员门户和用户门户中，单击门户设置并选择“删除所有设置和专用仪表板”的操作不正常。 此时会显示错误通知。 

<!-- 2930799 - IS ASDK --> 
- 在管理员门户和用户门户中的“所有服务”下，资产“DDoS 防护计划”未正确列出。 不是 Azure Stack 中提供。 如果尝试创建它，则会显示错误，指出门户无法创建此市场项。 

<!-- 2930820 - IS ASDK --> 
- 在管理员门户和用户门户中，如果搜索“Docker”，则此项无法正确返回。 不是 Azure Stack 中提供。 如果尝试创建它，则会显示一个边栏选项卡，其中包含表明存在错误的内容。 

<!-- 2967387 – IS, ASDK --> 
- 登录 Azure Stack 管理员门户或用户门户时使用的帐户显示为“未标识的用户”。 显示此消息，如果帐户不具有任一*第一个*或*最后一个*指定名称。 若要解决此问题，请编辑用户帐户，提供名或姓。 然后必须注销，再重新登录门户。  

<!--  2873083 - IS ASDK --> 
-  通过门户创建虚拟机规模集 (VMSS) 时，如果使用 Internet Explorer，则“实例大小”下拉列表无法正确加载。 若要解决此问题，请在通过门户创建 VMSS 时使用其他浏览器。  

<!-- 2931230 – IS  ASDK --> 
- 即使从用户订阅中删除计划，也无法删除作为附加计划添加到用户订阅的计划。 该计划将一直保留，直到引用附加计划的订阅也被删除。 

<!--2760466 – IS  ASDK --> 
- 安装运行此版本的新 Azure Stack 环境时，指示“需要激活”的警报可能不显示。 必须先[激活](azure-stack-registration.md)，然后才能使用市场联合。  

<!-- TBD - IS ASDK --> 
- 不应使用版本 1804 中引入的两种管理订阅类型。 这两种订阅类型为“计量订阅”和“消耗订阅”。 从版本 1804 开始，这些订阅类型会在新的 Azure Stack 环境中显示，但尚不可用。 请继续使用“默认提供程序”订阅类型。

<!-- TBD - IS ASDK --> 
- 删除用户订阅生成孤立的资源。 解决方法是先删除用户资源或整个资源组，然后再删除用户订阅。

<!-- TBD - IS ASDK --> 
- 无法使用 Azure Stack 门户查看订阅的权限。 解决方法是使用 PowerShell 验证权限。


### <a name="health-and-monitoring"></a>运行状况和监视

<!-- TBD - IS -->
- 可能会看到以下警报在 Azure Stack 系统上重复出现，然后消失：
   - *基础结构角色实例不可用*
   - *缩放单元节点已脱机*
   
  运行[Test-azurestack](azure-stack-diagnostic-test.md) cmdlet 来验证基础结构角色实例的运行状况和缩放单位节点。 如果 [Test-AzureStack](azure-stack-diagnostic-test.md) 未检测到问题，则可以忽略这些警报。 如果检测到问题，则可以尝试使用管理门户或 PowerShell 启动基础结构角色实例或节点。

  最新版本中修复此问题[1809年修补程序版本](https://support.microsoft.com/help/4477849/)，因此请确保安装此修补程序，如果遇到此问题。 

<!-- 1264761 - IS ASDK -->  
- 可能会看到包含以下详细信息的“运行状况控制器”组件的警报：  

   警报 #1：
   - 名称：基础结构角色不正常
   - 严重性：警告
   - 组件：运行状况控制器
   - 说明：运行状况控制器检测信号扫描仪不可用。 这可能会影响运行状况报告和指标。  

  警报 #2：
   - 名称：基础结构角色不正常
   - 严重性：警告
   - 组件：运行状况控制器
   - 说明：运行状况控制器故障扫描仪不可用。 这可能会影响运行状况报告和指标。

  可以放心忽略这两条警报，它们在一段时间后会自动关闭。  


<!-- 2812138 | IS --> 
- 可能会看到一个警报，以便**存储**组件具有下列详细信息：

   - 名称: 存储服务内部通信错误  
   - 严重性：严重  
   - 组件: 存储  
   - 说明: 将请求发送到以下节点时发生存储服务内部通信错误。  

    可以放心地忽略此警报，但需手动关闭它。

<!-- 2368581 - IS, ASDK --> 
- 如果 Azure Stack 操作员收到内存不足的警报，并且租户虚拟机无法部署并出现“Fabric VM 创建错误”，则可能表示 Azure Stack 模组的可用内存不足。 请使用 [Azure Stack 容量规划工具](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822)来充分了解可供工作负荷使用的容量。

### <a name="compute"></a>计算

<!-- 3235634 – IS, ASDK -->
- 若要将 Vm 部署包含的大小**v2**后缀; 例如， **Standard_A2_v2**，请指定作为后缀**Standard_A2_v2** (小写 v)。 不要使用**Standard_A2_V2** （大写 V）。 这适用于全球 Azure，Azure Stack 上的不一致。

<!-- 3099544 – IS, ASDK --> 
- 使用 Azure Stack 门户创建新的虚拟机 (VM) 并选择 VM 大小时，“美元/月”列在显示时会出现“不可用”消息。 此列不应显示；Azure Stack 不支持显示 VM 定价列。

<!-- 2869209 – IS, ASDK --> 
- 使用 [**Add-AzsPlatformImage** cmdlet](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0) 时，必须使用 **-OsUri** 参数作为存储帐户 URI（在其中上传磁盘）。 如果使用磁盘的本地路径，则此 cmdlet 会失败并显示以下错误：长时间运行的操作失败，状态为“失败”。 

<!--  2795678 – IS, ASDK --> 
- 通过门户创建“高级”VM 大小（DS、Ds_v2、FS、FSv2）的虚拟机 (VM) 时，该 VM 在标准存储帐户中创建。 在标准存储帐户中创建不影响功能、IOPS 或计费。 

   可以放心地忽略以下内容的警告：*在 VM 大小需要高级磁盘的情况下，你选择了使用标准磁盘。这可能影响操作系统性能，建议不要这样做。考虑改用高级存储(SSD)。*

<!-- 2967447 - IS, ASDK --> 
- 虚拟机规模集 (VMSS) 创建体验提供了基于 CentOS 7.2，作为一个选项来部署。 由于该映像不是可在 Azure Stack 上，选择为你的部署的另一个操作系统，或使用 Azure 资源管理器模板指定另一个已从 marketplace 部署前运算符所下载的 CentOS 映像。  

<!-- 2724873 - IS --> 
- 使用 PowerShell cmdlet **Start-AzsScaleUnitNode** 或 **Stop-AzsScaleunitNode** 管理缩放单元时，首次尝试启动或停止缩放单元可能会失败。 如果 cmdlet 在第一次运行时失败，请再次运行 cmdlet。 第二次运行应该能够成功地完成操作。 

<!-- TBD - IS ASDK --> 
- 如果在 VM 部署上预配某个扩展时耗时过长，用户应让预配超时，而不应尝试通过停止该进程来解除 VM 的分配或将 VM 删除。  

<!-- 1662991 IS ASDK --> 
- Azure Stack 不支持 Linux VM 诊断。 在部署启用 VM 诊断的 Linux VM 时，部署会失败。 如果通过诊断设置启用 Linux VM 的基本指标，部署也会失败。  

<!-- 2724961- IS ASDK --> 
- 注册时**Microsoft.Insight**订阅设置中的资源提供程序并创建 Windows VM 与来宾 OS 诊断已启用，在 VM 的概述页中的 CPU 百分比图表不会显示度量值数据。

   若要查找指标数据，例如 VM 的 CPU 百分比图表，请转到指标窗口并显示所有受支持的 Windows VM 来宾指标。



### <a name="networking"></a>网络  

<!-- 1766332 - IS ASDK --> 
- 如果在“网络”下单击“创建 VPN 网关”来设置 VPN 连接，则会将“基于策略”列为 VPN 类型。 请不要选择此选项。 Azure Stack 仅支持“基于路由”选项。

<!-- 1902460 - IS ASDK --> 
- Azure Stack 支持对一个 IP 地址使用一个本地网关。 这适用于所有租户订阅。 在创建第一个本地网关连接以后，系统会阻止使用同一 IP 地址创建本地网关资源的后续尝试。

<!-- 16309153 - IS ASDK --> 
- 在使用 DNS 服务器设置“自动”创建的虚拟网络上，无法更改为自定义 DNS 服务器。 更新的设置不推送到该 Vnet 中的 VM。

<!-- 2529607 - IS ASDK --> 
- 在 Azure Stack 机密轮换期间，有一个时段（两到五分钟）会无法访问公共 IP 地址。

<!-- 2664148 - IS ASDK --> 
-   在租户位置使用 S2S VPN 隧道访问其虚拟机的情况下，它们可能会遇到其中连接尝试失败，如果已创建网关后，在本地子网已添加到本地网络网关的方案。 


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>应用服务

<!-- 2352906 - IS ASDK --> 
- 在订阅中创建第一个 Azure 函数之前，用户必须先注册存储资源提供程序。


### <a name="usage"></a>使用情况  

<!-- TBD - IS ASDK --> 
- 公共 IP 地址使用计量数据显示相同*EventDateTime*而不是每个记录的值*TimeDate*演示创建记录时的时间戳。 目前，不能使用此数据执行的公共 IP 地址用量的准确计帐。


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>下载更新

可以从 Azure Stack 1809 更新包下载[此处](https://aka.ms/azurestackupdatedownload)。
  

## <a name="next-steps"></a>后续步骤

- 若要查看 Azure Stack 集成系统的服务策略，以及必须如何做才能使系统保持在受支持的状态，请参阅 [Azure Stack 服务策略](azure-stack-servicing-policy.md)。  
- 若要使用特权终结点 (PEP) 来监视和恢复更新，请参阅[使用特权终结点监视 Azure Stack 中的更新](azure-stack-monitor-update.md)。  
- 有关 Azure Stack 中更新管理的概述，请参阅[在 Azure Stack 中管理更新的概述](azure-stack-updates.md)。  
- 有关如何在 Azure Stack 中应用更新的详细信息，请参阅[在 Azure Stack 中应用更新](azure-stack-apply-updates.md)。  
