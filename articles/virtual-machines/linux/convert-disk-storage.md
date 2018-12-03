---
title: Azure 标准与高级托管磁盘存储的相互转换 | Microsoft Docs
description: 如何使用 Azure CLI 实现 Azure 标准与高级托管磁盘存储的相互转换。
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.openlocfilehash: 0d777b5dcebfba7dbff7c9ea1f4fedad12b3cf1a
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52283803"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Azure 标准与高级托管磁盘存储的相互转换

托管磁盘提供三种存储选项：[高级 SSD](../windows/premium-storage.md)、标准 SSD（预览）和[标准 HDD](../windows/standard-storage.md)。 它支持基于性能需求在选项之间轻松切换，并保障最短停机时间。 非托管磁盘不支持此操作。 但可以轻松[转换为托管磁盘](convert-unmanaged-to-managed-disks.md)，以在这些磁盘类型之间轻松切换。

本文介绍了如何使用 Azure CLI 实现 Azure 标准与高级托管磁盘存储的相互转换。 如果需要安装或升级它，请参阅[安装 Azure CLI](/cli/azure/install-azure-cli)。 

## <a name="before-you-begin"></a>开始之前

* 该转换需要重启 VM，因此请在预先存在的维护时段内计划磁盘存储迁移。 
* 如果使用非托管磁盘，请首先[转换为托管磁盘](convert-unmanaged-to-managed-disks.md)，并按照本文说明在存储选项之间切换。 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>实现 VM 所有标准与高级托管磁盘的相互转换

以下示例展示如何将 VM 的所有磁盘从标准存储切换到高级存储。 若要使用高级托管磁盘，VM 必须使用支持高级存储的 [VM 大小](sizes.md)。 此示例还切换到了支持高级存储的大小。

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the sku of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the sku of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>标准与高级托管磁盘的相互转换

对于开发/测试工作负荷，你可能想要同时具有标准磁盘和高级磁盘，以减少成本。 可通过仅将需要更佳性能的磁盘升级到高级存储来实现此目的。 以下示例展示如何将 VM 的单个磁盘在标准存储与高级存储之间相互切换。 若要使用高级托管磁盘，VM 必须使用支持高级存储的 [VM 大小](sizes.md)。 此示例还切换到了支持高级存储的大小。

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --ids $vmId --size $size

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>将托管磁盘在标准 HDD 与标准 SSD 之间相互转换

以下示例展示如何将 VM 的单个磁盘从标准 HDD 切换到标准 SSD。

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-using-the-azure-portal"></a>使用 Azure 门户进行转换

还可以使用 Azure 门户将非托管磁盘转换为托管磁盘。

1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 从门户的 VM 列表中选择 VM。
3. 在 VM 的边栏选项卡中，从菜单中选择“磁盘”。
4. 在“磁盘”边栏选项卡的顶部，选择“迁移到托管磁盘”。
5. 如果 VM 位于可用性集中，则“迁移到托管磁盘”边栏选项卡上会出现“首先需要转换可用性集”的警告。 此警告应该有一个链接，单击该链接即可转换可用性集。 转换可用性集后，或者如果 VM 不在可用性集中，请单击“迁移”以启动将磁盘迁移到托管磁盘的过程。 

VM 将会停止并在完成迁移后重新启动。

## <a name="next-steps"></a>后续步骤

使用[快照](snapshot-copy-managed-disk.md)获取 VM 的只读副本。

