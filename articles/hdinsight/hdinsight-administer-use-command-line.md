---
title: 使用 Azure 经典 CLI 管理 Apache Hadoop 群集 - Azure HDInsight
description: 了解如何使用 Azure 经典 CLI 管理 Azure HDInsight 中的 Apache Hadoop 群集。
services: hdinsight
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: tyfox
ms.openlocfilehash: 4de4674d8a4c2b573df12648739971e460531636
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495094"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-using-the-azure-classic-cli"></a>使用 Azure 经典 CLI 管理 HDInsight 中的 Apache Hadoop 群集
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

了解如何使用 [Azure 经典 CLI](../cli-install-nodejs.md) 管理 Azure HDInsight 中的 [Apache Hadoop](https://hadoop.apache.org/) 群集。 经典 CLI 是以 Node.js 实现的。 可以在支持 Node.js 的任意平台（包括 Windows、Mac 和 Linux）上使用它。

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

## <a name="prerequisites"></a>先决条件
在开始阅读本文前，必须具有：

* **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)。
* Azure 经典 CLI - 有关安装和配置信息，请参阅[安装和配置 Azure 经典 CLI](../cli-install-nodejs.md)。
* 使用以下命令**连接到 Azure**：

    ```cli
    azure login
    ```
  
    有关使用公司或学校帐户进行身份验证的详细信息，请参阅 [从 Azure 经典 CLI 连接到 Azure 订阅](/cli/azure/authenticate-azure-cli)。
* 使用以下命令**切换到 Azure 资源管理器模式**：
  
    ```cli
    azure config mode arm
    ```

若要获得帮助，请使用 **-h** 开关。  例如：

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>使用 CLI 创建群集
请参阅[使用 Azure 经典 CLI 在 HDInsight 中创建群集](hdinsight-hadoop-create-linux-clusters-azure-cli.md)。

## <a name="list-and-show-cluster-details"></a>列出并显示群集详细信息
使用以下命令来列出和显示群集详细信息：

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![群集列表的命令行视图][image-cli-clusterlisting]

## <a name="delete-clusters"></a>删除群集
使用以下命令来删除群集：

```cli
azure hdinsight cluster delete <Cluster Name>
```

还可通过删除包含该群集的资源组来删除群集。 请注意，这会删除组中的所有资源（包括默认存储帐户）。

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>缩放群集
若要更改 Apache Hadoop 群集大小，请执行以下命令：

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>启用/禁用对群集的 HTTP 访问

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="next-steps"></a>后续步骤
在本文中，已了解如何执行不同的 HDInsight 群集管理任务。 若要了解更多信息，请参阅下列文章：

* [使用 Azure 门户管理 HDInsight][hdinsight-admin-portal]
* [使用 Azure PowerShell 管理 HDInsight][hdinsight-admin-powershell]
* [Azure HDInsight 入门][hdinsight-get-started]
* [如何使用 Azure 经典 CLI][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "列出并显示集群"
