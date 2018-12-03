---
title: Azure 备份：准备备份虚拟机
description: 确保对环境进行准备，以便在 Azure 中备份虚拟机。
services: backup
author: rayne-wiselman
manager: carmonm
keywords: 备份; 正在备份;
ms.service: backup
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: raynew
ms.openlocfilehash: 1092f5e21eab1e037c360408f17548b544a9e922
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422790"
---
# <a name="prepare-to-back-up-azure-vms"></a>准备备份 Azure VM

本文提供有关准备环境以备份 Azure 资源管理器部署的虚拟机 (VM) 的步骤。 过程中显示的步骤将使用 Azure 门户。 备份虚拟机时，备份数据或恢复点存储在恢复服务备份保管库。



在保护（或备份）资源管理器部署虚拟机之前，请确保符合以下先决条件：

* 在虚拟机所在区域创建或标识恢复服务保管库。
* 选择方案、定义备份策略并定义要保护的项。
* 检查虚拟机上的 VM 代理（扩展）的安装情况。
* 检查网络连接。
* 对于 Linux VM，如果要自定义备份环境以进行应用程序一致的备份，请按照[配置快照前和快照后脚本的步骤](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)进行操作。

如果环境已满足这些条件，请转到[备份 VM](backup-azure-arm-vms.md) 一文。 如果需要设置或检查上述任何先决条件，本文将逐步引导你完成各个步骤。

## <a name="supported-operating-systems-for-backup"></a>支持用于备份的操作系统

 * **Linux**：Azure 备份支持 [Azure 认可的分发版列表](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)，但 CoreOS Linux 和 32 位操作系统除外。 有关支持还原文件的 Linux 操作系统的列表，请参阅[从虚拟机备份恢复文件](backup-azure-restore-files-from-vm.md#for-linux-os)。

    > [!NOTE]
    > 只要虚拟机上装有 VM 代理且支持 Python，其他自带 Linux 发行版应该也能正常运行。 但是，不支持这些发行版。
    >
 * **Windows Server**、**Windows 客户端**：不支持低于 Windows Server 2008 R2 或 Windows 7 的版本。


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>备份和还原 VM 时的限制
准备环境之前，请务必了解限制：

* 不支持备份超过 16 个数据磁盘的虚拟机。
* 不支持备份通过 Linux 统一密钥设置 (LUKS) 加密法加密的 Linux VM。
* 不建议备份包含群集共享卷 (CSV) 或横向扩展文件服务器配置的 VM。 如果已备份，会造成 CSV 编写器故障。 这些操作涉及到在执行快照任务执行期间包含在群集配置中的所有 VM。 Azure 备份不支持多 VM 一致性。
* 备份数据不包括连接到 VM 的网络挂载驱动器。
* “还原配置”中的“替换现有” 选项有助于将当前 VM 中的现有磁盘替换为所选的还原点。 仅在当前 VM 存在时，才可执行此操作。 
* 不支持跨区域备份和还原。
* 配置备份时，请确保“防火墙和虚拟网络”存储帐户设置允许从“所有网络”进行访问。
* 对于所选的网络，为你的存储帐户配置防火墙和虚拟网络设置后，请选择“允许受信任的 Microsoft 服务访问此存储帐户”作为例外，以允许 Azure 备份服务访问网络受限的存储帐户。 网络受限的存储帐户不支持项级别的恢复。
* 可以在 Azure 的所有公共区域中备份虚拟机。 （请参阅支持区域的[清单](https://azure.microsoft.com/regions/#services)。）在创建保管库期间，如果要寻找的区域目前不受支持，则不会在下拉列表中显示它。
* 仅支持通过 PowerShell 还原属于多 DC 配置的域控制器 (DC) VM。 有关详细信息，请参阅[还原多 DC 域控制器](backup-azure-arm-restore-vms.md#restore-domain-controller-vms)。
* 不支持已启用写入加速器的磁盘上的快照。 此限制会导致 Azure 备份服务无法对虚拟机的所有磁盘执行应用程序一致的快照。
* Azure 备份不支持自动调整时钟以实现用于备份 Azure VM 的夏令时更改。 如果需要，请修改策略以将夏令时更改考虑在内。
* 仅支持通过 PowerShell 还原采用以下特殊网络配置的虚拟机。 还原操作完成后，在 UI 中通过还原工作流创建的 VM 将不采用这些网络配置。 若要了解详细信息，请参阅[还原采用特殊网络配置的 VM](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations)。
  * 采用负载均衡器配置的虚拟机（内部和外部）
  * 使用多个保留 IP 地址的虚拟机
  * 使用多个网络适配器的虚拟机

  > [!NOTE]
  > Azure 备份支持[标准 SSD 托管磁盘](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/)，该磁盘是 Microsoft Azure 虚拟机的一种新型持久存储器。 [Azure VM 备份堆栈 V2](backup-upgrade-to-vm-backup-stack-v2.md) 支持托管磁盘。

## <a name="create-a-recovery-services-vault-for-a-vm"></a>为 VM 创建恢复服务保管库
恢复服务保管库是用于存储在不同时间创建的备份和恢复点的实体。 恢复服务保管库还包含与受保护虚拟机关联的备份策略。

若要创建恢复服务保管库，请执行以下操作：

1. 登录到 [Azure 门户](https://portal.azure.com/)。
1. 在“中心”菜单中选择“浏览”，并键入“恢复服务”。 开始键入时，会根据输入筛选资源列表。 选择“恢复服务保管库”。

    ![在框中键入内容，并在结果中选择“恢复服务保管库”](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    此时会显示恢复服务保管库列表。
1. 在“恢复服务保管库”菜单中，选择“添加”。

    ![创建恢复服务保管库步骤 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    此时会打开“恢复服务保管库”窗格。 此窗格中会提示输入“名称”、“订阅”、“资源组”和“位置”的信息。

    ![“恢复服务保管库”窗格](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
1. 对于“名称”，请输入一个友好名称以标识保管库 。 名称对于 Azure 订阅需要是唯一的。 键入包含 2 到 50 个字符的名称。 名称必须以字母开头，只能包含字母、数字和连字符。
1. 选择“订阅”查看可用订阅列表。 如果不确定要使用哪个订阅，请使用默认的（或建议的）订阅。 仅当工作或学校帐户与多个 Azure 订阅关联时，才会显示多个选项。
1. 选择“资源组”查看可用资源组列表，或选择“新建”创建新的资源组。 有关资源组的完整信息，请参阅 [Azure 资源管理器概述](../azure-resource-manager/resource-group-overview.md)。
1. 选择“位置”，为保管库选择地理区域。 *必须* 与要保护的虚拟机位于同一区域中。

   > [!IMPORTANT]
   > 如果不确定 VM 的所在位置，请关闭保管库创建对话框，并转到门户中的虚拟机列表。 如果在多个区域中具有虚拟机，则需要在每个区域中创建恢复服务保管库。 请先在第一个位置创建保管库，然后转到下一个位置。 无需指定存储帐户即可存储备份数据。 恢复服务保管库和 Azure 备份服务会自动处理这种情况。
   >
   >

1. 选择“创建”。 创建恢复服务保管库可能需要一段时间。 可以在门户的右上区域中监视状态通知。 创建保管库后，它会显示在“恢复服务保管库”的列表中。 如果未看到创建的保管库，请选择“刷新”。

    ![备份保管库列表](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

现已创建保管库，接下来请了解如何设置存储复制。

## <a name="set-storage-replication"></a>设置存储复制
使用存储复制选项可在异地冗余存储与本地冗余存储之间进行选择。 默认情况下，保管库具有异地冗余存储。 对于主要备份，请将选项保持设置为异地冗余存储。 如果想要一个更便宜、但持久性不高的选项，请选择“本地冗余存储”。

若要编辑存储复制设置，请执行以下操作：

1. 在“恢复服务保管库”窗格中，选择自己的保管库。
    选择该保管库时，“设置”窗格（顶部显示了保管库的名称）和保管库详细信息窗格会打开。

   ![从备份保管库列表中选择保管库](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

1. 在“设置”窗格中，使用垂直滚动条向下滚动到“管理”部分，并选择“备份基础结构”。 在“常规”部分中，选择“备份配置”。 在“备份配置”窗格中，选择保管库的存储复制选项。 默认情况下，保管库具有异地冗余存储。

   ![备份保管库列表](./media/backup-azure-arm-vms-prepare/full-blade.png)

   如果使用 Azure 作为主要备份存储终结点，请继续使用异地冗余存储。 如果使用 Azure 作为非主要备份存储终结点，请选择本地冗余存储。 请参阅 [Azure 存储复制概述](../storage/common/storage-redundancy.md)详细了解存储选项。

1. 如果更改了存储复制类型，请选择“保存”。

选择保管库的存储选项后，可以开始将 VM 与保管库相关联。 若要开始关联，请发现及注册 Azure 虚拟机。

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>选择备份目标、设置策略并定义要保护的项
向恢复服务保管库注册虚拟机之前，请运行发现过程以确定是否有任何新的虚拟机已添加到订阅。 发现过程将在 Azure 中查询订阅中的虚拟机列表。 如果找到新的虚拟机，门户将显示云服务名称和关联的区域。 在 Azure 门户中，*方案*是你在恢复服务保管库中输入的内容。 策略是有关恢复点创建频率和时间的计划。 策略还包含恢复点的保留范围。

1. 如果已打开恢复服务保管库，请转到步骤 2。 如果尚未打开恢复服务保管库，请打开 [Azure 门户](https://portal.azure.com/)。 在“中心”菜单中，选择“更多服务”。

   a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在资源列表中，键入“恢复服务”。 开始键入时，会根据输入筛选列表。 出现“**恢复服务保管库**”时，请选择它。

      ![在框中键入内容，并在结果中选择“恢复服务保管库”](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      此时会显示恢复服务保管库列表。 如果订阅中没有任何保管库，则此列表为空。

      ![恢复服务保管库列表的视图](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   b. 在恢复服务保管库列表中选择一个保管库。

      此时会打开所选保管库的“设置”窗格和保管库仪表板。

      ![“设置”窗格和保管库仪表板](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
1. 在保管库仪表板菜单中，选择“备份”。

   ![“备份”按钮](./media/backup-azure-arm-vms-prepare/backup-button.png)

   此时会打开“备份”和“备份目标”窗格。

1. 在“备份目标”窗格中，将“工作负荷的运行位置”设置为“Azure”，并将“要备份的项”设置为“虚拟机”。 然后选择“确定”。

   ![“备份”和“备份目标”窗格](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   此步骤向保管库注册 VM 扩展。 随后将会关闭“备份目标”窗格，并打开“备份策略”窗格。

   ![“备份”和“备份策略”窗格](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
1. 在“备份策略”窗格中，选择要应用到保管库的备份策略。

   ![选择备份策略](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   默认策略的详细信息会在下拉菜单下列出。 如果要创建新策略，请从下拉菜单中选择“新建”。 有关定义备份策略的说明，请参阅[定义备份策略](backup-azure-vms-first-look-arm.md#defining-a-backup-policy)。
    选择“确定”，将备份策略与保管库相关联。

   随后将会关闭“备份策略”窗格，并打开“选择虚拟机”窗格。
1. 在“选择虚拟机”窗格中，选择要与指定的策略关联的虚拟机，并选择“确定”。

   ![“选择虚拟机”窗格](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   所选虚拟机已验证。 如果未看到所需的虚拟机，请检查这些虚拟机是否位于恢复服务保管库所在的同一个 Azure 区域。 如果仍看不到虚拟机，请检查它们是否尚未受其他保管库保护。 保管库仪表板将显示恢复服务保管库所在的区域。

1. 定义保管库的所有设置后，请在“备份”窗格中选择“启用备份”。 此步骤将策略部署到保管库和 VM。 此步骤不会创建虚拟机的初始恢复点。

   ![“启用备份”按钮](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

成功启用备份后，备份策略将按计划运行。 如果现在想要生成按需备份来备份虚拟机，请参阅[触发备份作业](./backup-azure-vms-first-look-arm.md#initial-backup)。

如果注册虚拟机出现问题，请参阅以下信息，了解安装 VM 代理的方法和网络连接的相关信息。 如果要保护在 Azure 中创建的虚拟机，则可能不需要以下信息。 但是，如果已将虚拟机迁移到 Azure，请确保已正确安装 VM 代理，并且虚拟机可与虚拟网络通信。

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>在虚拟机上安装 VM 代理
要正常运行备份扩展，必须在 Azure 虚拟机上安装 Azure [VM 代理](../virtual-machines/extensions/agent-windows.md)。 如果 VM 是从 Azure 市场创建的，则虚拟机上已安装 VM 代理。

以下信息适用于不是使用从 Azure 市场创建的 VM 的情况。 **例如，你从本地数据中心迁移了某个 VM。在这种情况下，需要安装 VM 代理才能保护该虚拟机。**

**请注意**：安装 VM 代理以后，还必须使用 Azure PowerShell 更新 ProvisionGuestAgent 属性，以告知 Azure VM 已安装代理。

如果在备份 Azure VM 时遇到问题，请使用下表检查是否已在虚拟机上正确安装 Azure VM 代理。 该表提供了适用于 Windows 和 Linux VM 的 VM 代理的其他信息。

| **操作** | **Windows** | **Linux** |
| --- | --- | --- |
| 安装 VM 代理 |下载并安装 [代理 MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)。 需要有管理员权限才能完成安装。 |<li> 安装最新的 [Linux 代理](../virtual-machines/extensions/agent-linux.md)。 需要有管理员权限才能完成安装。 我们建议从分发存储库安装代理。 我们**不建议**直接从 github 安装 Linux VM 代理。  |
| 更新 VM 代理 |更新 VM 代理与重新安装 [VM 代理二进制文件](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)一样简单。 <br>确保在更新 VM 代理时，没有任何正在运行的备份操作。 |按照[更新 Linux VM 代理](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)中的说明进行操作。 我们建议从分发存储库更新代理。 我们**不建议**直接从 github 更新 Linux VM 代理。<br>确保在更新 VM 代理时，没有任何正在运行的备份操作。 |
| 验证 VM 代理安装 |<li>导航到 Azure VM 中的 *C:\WindowsAzure\Packages* 文件夹。 <li>应会发现 WaAppAgent.exe 文件已存在。<li> 右键单击该文件，转到“**属性**”，并选择“**详细信息**”选项卡。“产品版本”字段应为 2.6.1198.718 或更高。 |不适用 |

### <a name="backup-extension"></a>备份扩展
在虚拟机上安装 VM 代理后，Azure 备份服务会将备份扩展安装到 VM 代理上。 备份服务会无缝升级和修补备份扩展。

无论 VM 是否在运行，备份服务都会安装备份扩展。 VM 运行时，很有可能会获得应用程序一致的恢复点。 但是，即使 VM 已关闭且无法安装扩展，备份服务也会继续备份 VM。 此类 VM 称为“脱机 VM”。 在这种情况下，恢复点将是 *崩溃一致性*恢复点。

## <a name="establish-network-connectivity"></a>建立网络连接
为了管理 VM 快照，备份扩展需要连接 Azure 公共 IP 地址。 如果未建立适当的 Internet 连接，虚拟机的 HTTP 请求会超时，并且备份操作会失败。 如果部署中配置了访问限制（例如，通过网络安全组 (NSG)），请选择其中一个选项来提供备份流量的明确路径：

* [将 Azure 数据中心 IP 范围加入允许列表](https://www.microsoft.com/download/details.aspx?id=41653)。
* 部署 HTTP 代理服务器来路由流量。

在确定使用哪个选项时，要取舍的不外乎是易管理性、控制粒度和成本等要素。

| 选项 | 优点 | 缺点 |
| --- | --- | --- |
| 将 IP 范围加入允许列表 |无额外成本。<br><br>若要在 NSG 中打开访问权限，请使用 **Set-AzureNetworkSecurityRule** cmdlet。 |管理起来很复杂，因为受影响的 IP 范围会不断变化。<br><br>允许访问整个 Azure，而不只是存储。 |
| 使用 HTTP 代理 |允许在代理中对存储 URL 进行精细控制。<br><br>在单个位置通过 Internet 访问 VM。<br><br>不受 Azure IP 地址变化的影响。 |通过代理软件运行 VM 带来的额外成本。 |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>将 Azure 数据中心 IP 范围加入允许列表
要将 Azure 数据中心 IP 范围加入允许列表，请参阅 [Azure 网站](https://www.microsoft.com/download/details.aspx?id=41653)获取有关 IP 范围的详细信息和说明。

可以允许使用[服务标记](../virtual-network/security-overview.md#service-tags)与特定区域的存储建立连接。 请确保允许访问存储帐户的规则的优先级高于阻止 Internet 访问的规则。

![具有区域存储标记的 NSG](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

以下视频逐步讲解了配置服务标记的过程：

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!NOTE]
> 有关存储服务标记和区域的列表，请参阅[存储的服务标记](../virtual-network/security-overview.md#service-tags)。

### <a name="use-an-http-proxy-for-vm-backups"></a>使用 HTTP 代理进行 VM 备份
备份 VM 时，VM 上的备份扩展会使用 HTTPS API 将快照管理命令发送到 Azure 存储。 将通过 HTTP 代理路由备份扩展流量，因为它是为了访问公共 Internet 而配置的唯一组件。

> [!NOTE]
> 至于应该使用何种代理软件，我们不提供任何建议。 请确保能够对选择的代理执行下述配置步骤。
>
>

以下示例图像显示了使用 HTTP 代理所要执行的三个配置步骤：

* 应用 VM 通过代理 VM 路由所有发往公共 Internet 的 HTTP 流量。
* 代理 VM 允许来自虚拟网络中 VM 的传入流量。
* 名为 NSF-lockdown 的网络安全组需要使用一个安全规则允许来自代理 VM 的出站 Internet 流量。

若要使用 HTTP 代理来与公共 Internet 通信，请完成以下步骤。

> [!NOTE]
> 这些步骤使用本示例中的特定名称和值。 将详细信息输入（或粘贴）到代码中时，请使用部署的名称和值。

#### <a name="step-1-configure-outgoing-network-connections"></a>步骤 1：配置传出网络连接
###### <a name="for-windows-machines"></a>对于 Windows 计算机
此过程将设置本地系统帐户的代理服务器配置。

1. 下载 [PsExec](https://technet.microsoft.com/sysinternals/bb897553)。
1. 在权限提升的提示符下运行以下命令，打开 Internet Explorer：

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

1. 在 Internet Explorer 中，转到“工具” > “Internet 选项” > “连接” > “局域网设置”。
1. 验证系统帐户的代理设置。 设置代理 IP 和端口。
1. 关闭 Internet Explorer。

以下脚本设置计算机范围的代理配置，可将该配置用于任何传出的 HTTP 或 HTTPS 流量。 如果已在当前用户帐户（非本地系统帐户）中设置代理服务器，请使用此脚本将设置应用到 SYSTEMACCOUNT。

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> 如果在代理服务器日志中发现“(407)需要代理身份验证”，请检查身份验证设置是否正确。
>
>

###### <a name="for-linux-machines"></a>对于 Linux 计算机
将以下代码行添加到 ```/etc/environment``` 文件：

```
http_proxy=http://<proxy IP>:<proxy port>
```

将以下代码行添加到 ```/etc/waagent.conf``` 文件：

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>步骤 2：在代理服务器上允许传入连接
1. 在代理服务器上打开 Windows 防火墙。 访问防火墙的最简单方法是搜索“高级安全 Windows 防火墙”。
1. 在“高级安全 Windows 防火墙”对话框中，右键单击“入站规则”并选择“新建规则”。
1. 在“新建入站规则向导”中的“规则类型”页上，选择“自定义”选项，然后选择“下一步”。
1. 在“程序”页上，依次选择“所有程序”、“下一步”。
1. 在“协议和端口”页上输入以下信息，选择“下一步”：
   * 对于“协议类型”，请选择“TCP”。
   * 对于“本地端口”，请选择“特定端口”。 在随后显示的框中，指定已配置的代理端口号。
   * 对于“远程端口”，请选择“所有端口”。

在向导的余下部分中，请一路接受默认设置。 然后为此规则命名。

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>步骤 3：向 NSG 添加例外规则
以下命令会在 NSG 中添加一个例外。 此例外允许从 10.0.0.5 上的任何端口流向端口 80 (HTTP) 或 443 (HTTPS) 上的任何 Internet 地址的 TCP 流量。 如果需要访问公共 Internet 中的特定端口，请务必将该端口添加到 ```-DestinationPortRange```。

在 Azure PowerShell 命令提示符下输入以下命令：

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>有疑问？
如有疑问或希望包含某种功能，请[向我们发送反馈](https://aka.ms/azurebackup_feedback)。

## <a name="next-steps"></a>后续步骤
现在已准备好环境来备份 VM，下一个逻辑步骤是创建备份。 规划文章提供了有关备份 VM 的更详细信息。

* [备份虚拟机](backup-azure-arm-vms.md)
* [规划 VM 备份基础结构](backup-azure-vms-introduction.md)
* [管理虚拟机备份](backup-azure-manage-vms.md)
