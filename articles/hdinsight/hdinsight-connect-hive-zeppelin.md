---
title: 在 Azure HDInsight 中使用 Apache Zeppelin 运行 Apache Hive 查询
description: 了解如何使用 Apache Zeppelin 运行 Apache Hive 查询。
keywords: hdinsight,hadoop,hive,交互式查询,LLAP
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 035e70eef88d5d5dae08c329017430db25c20464
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494825"
---
# <a name="use-apache-zeppelin-to-run-apache-hive-queries-in-azure-hdinsight"></a>在 Azure HDInsight 中使用 Apache Zeppelin 运行 Apache Hive 查询 

HDInsight 交互式查询群集包括可用来运行交互式 Hive 查询的 [Apache Zeppelin](https://zeppelin.apache.org/) 笔记本。 本文介绍如何使用 Apache Zeppelin 在 Azure HDInsight 中运行 [Apache Hive](https://hive.apache.org/) 查询。 

## <a name="prerequisites"></a>先决条件
在开始阅读本文前，必须具备以下项：

* **HDInsight 交互式查询群集**。 若要创建 HDInsight 群集，请参阅[创建群集](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster)。  请确保选择“交互式查询”类型。 

## <a name="create-an-apache-zeppelin-note"></a>创建 Apache Zeppelin 笔记

1. 浏览到以下 URL：

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    将 **CLUSTERNAME** 替换为群集名称。

2. 输入 Hadoop 用户名和密码。 在 Zeppelin 页中，可以创建新笔记，也可以打开现有笔记。 HiveSample 包含一些示例 Hive 查询。  

    ![HDInsight 交互式查询 zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. 单击“创建新笔记”。
4. 键入或选择以下值：

    - 笔记名称：输入笔记的名称。
    - 默认解释器：选择 **JDBC**。

5. 单击“创建笔记”。
6. 运行以下 Hive 查询：

        %jdbc(hive)
        show tables

    ![HDInsight 交互式查询 zeppelin 运行查询](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    第一行中的 **%jdbc(hive)** 语句告诉笔记本使用 Hive JDBC 解释程序。

    该查询将返回一个名为 *hivesampletable* 的 Hive 表。

    以下是可以针对 hivesampletable 运行的附加的两个 Hive 查询。 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    与传统 Hive 相比，返回查询结果的速度更快。


## <a name="next-steps"></a>后续步骤
本文介绍了如何使用 [Microsoft Power BI](https://powerbi.microsoft.com/) 直观显示 HDInsight 中的数据。  若要了解更多信息，请参阅下列文章：

* [在 Azure HDInsight 中使用 Microsoft Power BI 直观显示 Apache Hive 数据](hadoop/apache-hadoop-connect-hive-power-bi.md)。
* [在 Azure HDInsight 中使用 Power BI 直观显示交互式查询 Apache Hive 数据](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)。
* [使用 Microsoft Hive ODBC 驱动程序将 Excel 连接到 HDInsight](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)。
* [使用 Power Query 将 Excel 连接到 Apache Hadoop](hadoop/apache-hadoop-connect-excel-power-query.md)。
* [使用针对 Visual Studio 的 Data Lake 工具连接到 Azure HDInsight 并运行 Apache Hive 查询](hadoop/apache-hadoop-visual-studio-tools-get-started.md)。
* [使用用于 Visual Studio Code 的 Azure HDInsight 工具](hdinsight-for-vscode.md)。
* [将数据上传到 HDInsight](./hdinsight-upload-data.md)。
