---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/25/2018
ms.author: cynthn
ms.openlocfilehash: 7c9c52c5cda258908ac8463dd06044e244c75ee2
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52330754"
---
不再需要附加到虚拟机 (VM) 的数据磁盘时，可以轻松地分离它。 从 VM 中分离磁盘时，不会从存储中删除该磁盘。 如果希望再次使用磁盘上的现有数据，可以将其重新附加到相同的 VM 或另一个 VM。  

> [!NOTE]
> Azure 中的 VM 使用不同类型的磁盘 - 操作系统磁盘、本地临时磁盘和可选数据磁盘。 有关详细信息，请参阅[关于虚拟机的磁盘和 VHD](../articles/virtual-machines/linux/about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。 除非同时也删除 VM，否则无法分离操作系统磁盘。

## <a name="find-the-disk"></a>找到磁盘
在从 VM 中分离磁盘之前，需要先确定 LUN 号（要分离的磁盘的标识符）。 为此，请执行以下步骤：

1. 打开 Azure CLI 并[连接到 Azure 订阅](/cli/azure/authenticate-azure-cli)。 确保是在 Azure 服务管理模式 (`azure config mode asm`) 下。
2. 找出哪些磁盘已附加到 VM。 以下示例列出名为 `myVM` 的 VM 的磁盘：

    ```azurecli
    azure vm disk list myVM
    ```

    输出类似于以下示例：

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. 请记录要分离的磁盘的 LUN 或**逻辑单元号**。

## <a name="remove-operating-system-references-to-the-disk"></a>删除对该磁盘的操作系统引用
从 Linux 来宾分离磁盘之前，应确保磁盘上的所有分区都未在使用。 确保操作系统在重新启动后不会尝试重新装载它们。 这些步骤将撤消在[附加](../articles/virtual-machines/linux/classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)磁盘时有可能创建的配置。

1. 使用 `lsscsi` 命令发现磁盘标识符。 `lsscsi` 的安装可以通过 `yum install lsscsi`（在基于 Red Hat 的分发版上）或 `apt-get install lsscsi`（在基于 Debian 的分发版上）来进行。 可以使用 LUN 号找到要寻找的磁盘标识符。 每一行的元组中的最后一个数字就是 LUN。 在下面的示例中，从 `lsscsi` LUN 0 映射到 */dev/sdc*

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. 使用 `fdisk -l <disk>` 找到与要分离的磁盘关联的分区。 以下示例显示 `/dev/sdc` 的输出：

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. 卸载磁盘列出的每个分区。 以下示例卸载 `/dev/sdc1`：

    ```bash
    sudo umount /dev/sdc1
    ```

4. 使用 `blkid` 命令找到所有分区的 UUID。 输出类似于以下示例：

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. 删除与要分离的磁盘的所有分区的设备路径或 UUID 关联的 **/etc/fstab** 文件中的条目。  此示例的条目可能是：

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    或
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-the-disk"></a>分离磁盘
找到磁盘的 LUN 号并删除操作系统引用后，就可以将其分离：

1. 通过运行命令 `azure vm disk detach
   <virtual-machine-name> <LUN>` 从虚拟机中分离所选磁盘。 以下示例将 LUN `0` 从名为 `myVM` 的 VM 中分离：
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. 可以再次运行 `azure vm disk list` 来检查该磁盘是否已分离。 下面的示例检查名为 `myVM` 的 VM：
   
    ```azurecli
    azure vm disk list myVM
    ```

    输出类似于以下示例，该示例显示数据磁盘不再附加：

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

分离的磁盘保留在存储中，但不再附加到虚拟机。

