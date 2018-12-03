---
title: Apache Hadoop 组件和版本 - Azure HDInsight
description: 了解 HDInsight 中的 Apache Hadoop 组件和版本，以及 Hortonworks 数据平台的此云分发版中可用的服务级别。
keywords: hadoop 版本,hadoop 生态系统组件,hadoop 组件,如何检查 hadoop 版本
services: hdinsight
ms.reviewer: jasonh
author: kkampf
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: kakampf
ms.openlocfilehash: 2ae0450be420edeb4ef18d0c9e5ca6e1dacaf50d
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52497012"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>HDInsight 提供了哪些 Apache Hadoop 组件和版本？

了解 Microsoft Azure HDInsight 中的 [Apache Hadoop](https://hadoop.apache.org/) 生态系统组件和版本，以及企业安全性套餐。 另外，还将了解如何检查 HDInsight 中的 Hadoop 组件版本。 

每个 HDInsight 版本都是某个版本的 Hortonworks 数据平台 (HDP) 的云分发版。

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>随不同 HDInsight 版本提供的 Apache Hadoop 组件
Azure HDInsight 支持多个可随时部署的 Hadoop 群集版本。 每个版本选项创建 HDP 分发版的一个特定版本和该分发版内包含的一组组件。 从 2017 年 4 月 4 日起，Azure HDInsight 使用的默认群集版本是 3.6 并基于 HDP 2.6。

下表列出了与 HDInsight 群集版本关联的组件版本： 

> [!NOTE]
> HDInsight 服务的默认版本可能会更改，不会另行通知。 如果依赖某个版本，建议在使用 .NET SDK 以及 Azure PowerShell 和 Azure 经典 CLI 创建群集时指定 HDInsight 版本。

| 组件 | HDInsight 4.0（预览版） | HDInsight 3.6（默认） | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 | HDInsight 3.1 | HDInsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks 数据平台 |3.0 |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop 和 YARN |3.1.1 |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.9.1 |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive 和 HCatalog |-|1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive |3.1.0 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 |-| 0.8.4 |-|-|-|-|-|-|
| Apache Ranger |1.1.0 |0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |2.0.1 |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.7 |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.3.1 |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.2.1 |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |-|0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |5 |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.3.1 |2.3.0、2.2.0、2.1.0 |1.6.2, 2.0 |1.6.0 |1.5.2 |1.3.1（仅限 Windows） |-|-|
| Apache Livy |0.5 |0.4 |0.3 |0.3 |0.2 |-|-|-|
| Apache Kafka | 1.1 |1.1, 1.0, 0.10.1 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.7.0 |2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.8.0 |0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |4.2.1 |3.2.8 |-|-|-|
| Apache 滑块 |-| 0.92.0 |-|-|-|-|-|-|

## <a name="check-for-current-hadoop-component-version-information"></a>检查当前的 Hadoop 组件版本信息

与 HDInsight 群集版本关联的组件版本可能会随 HDInsight 的更新而更改。 若要检查 Hadoop 组件并验证正在为群集使用哪些版本，请使用 Ambari REST API。 **GetComponentInformation** 命令检索有关服务组件的信息。 有关详细信息，请参阅 [Apache Ambari 文档][ambari-docs]。

对于 Windows 群集，检查组件版本的另一种方法是使用远程桌面登录到群集并检查 C:\apps\dist\ 目录的内容。

> [!IMPORTANT]
> Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 上的 Windows 停用](#hdinsight-windows-retirement)。

### <a name="release-notes"></a>发行说明

请参阅 [HDInsight 发行说明](hdinsight-release-notes.md)，了解 HDInsight 最新版本的更多发行说明。

## <a name="supported-hdinsight-versions"></a>支持的 HDInsight 版本
下表列出了 HDInsight 版本。 其中列出了与每个 HDInsight 版本对应的 HDP 版本以及产品发行日期。 还提供了支持到期日期和停用日期（如果已知这些日期）。

### <a name="available-versions"></a>可用版本

下表列出了 Azure 门户中可用的 HDInsight 版本以及其他部署方法（比如 PowerShell 和 .NET SDK）。

| HDInsight 版本 | HDP 版本 | VM OS | 发行日期 | 支持到期日期 | 停用日期 | 高可用性 |  在 Azure 门户上的可用性 | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 <br> （预览版） |HDP 3.0 |Ubuntu 16.0.4 LTS |2018 年 9 月 24 日 | | |是 |是 |
| HDInsight 3.6 |HDP 2.6 |Ubuntu 16.0.4 LTS |2017 年 4 月 4 日 | | |是 |是 |
| HDInsight 3.5 <br> (Spark)* |HDP 2.6 |Ubuntu 16.0.4 LTS |2016 年 9 月 30 日 |2019 年 3 月 13 日 |2019 年 3 月 13 日 |是 |是 |

*&ast;HDInsight 3.5 支持仅针对 Spark 群集类型进行了扩展*

> [!NOTE]
> 在对某个版本的支持到期后，不能通过 Microsoft Azure 门户获得该版本。 但是，可继续使用 Windows PowerShell [New-AzureRmHDInsightCluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightcluster) 命令中的 `Version` 参数和 .NET SDK 获取群集版本，直到版本停用的那天为止。
>

### <a name="retired-versions"></a>已停用的版本

下表列出了 Azure 门户中**不**可用的 HDInsight 版本。

| HDInsight 版本 | HDP 版本 | VM OS | 发行日期 | 支持到期日期 | 停用日期 | 高可用性 |  在 Azure 门户上的可用性 | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 <br> （非 Spark） |HDP 2.5 |Ubuntu 16.0.4 LTS |2016 年 9 月 30 日 |2017 年 9 月 5 日 |2018 年 6 月 28 日 |是 |否 |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |2016 年 3 月 29 日 |2016 年 12 月 29 日 |2018 年 1 月 9 日 |是 |否 |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2015 年 12 月 2 日 |2016 年 6 月 27 日 |2018 年 7 月 31 日 |是 |否 |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2015 年 12 月 2 日 |2016 年 6 月 27 日 |2017 年 7 月 31 日 |是 |否 |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS 或 Windows Server 2012 R2 |2015 年 2 月 18 日 |2016 年 3 月 1 日 |2017 年 4 月 1 日 |是 |否 |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |2014 年 6 月 24 日 |2015 年 5 月 18 日 |2016 年 6 月 30 日 |是 |否 |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |2014 年 2 月 11 日 |2014 年 9 月 17 日 |2015 年 6 月 30 日 |是 |否 |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |2013 年 10 月 28 日 |2014 年 5 月 12 日 |2015 年 5 月 31 日 |是 |否 |
| HDInsight 1.6 |HDP 1.1 | |2013 年 10 月 28 日 |2014 年 4 月 26 日 |2015 年 5 月 31 日 |否 |否 |

> [!NOTE]
> 对于 HDInsight 2.1 和更高版本，默认情况下会集部署具有两个头节点的高度可用群集。 它们不适用于 HDInsight 1.6 版本的群集。

## <a name="enterprise-security-package-for-hdinsight"></a>适用于 HDInsight 的 Enterprise Security 包

Enterprise Security 是一种可选包，可在 HDInsight 群集上作为创建群集工作流的一部分添加。 Enterprise Security 包支持：

- 与 Active Directory 集成进行身份验证。

    在过去，只能创建包含本地管理员用户和本地 SSH 用户的 HDInsight 群集。 本地管理员用户可以访问所有文件、文件夹、表和列。  如果使用 Enterprise Security 包，可通过将 HDInsight 群集与你自己的 Active Directory（包括本地 Active Directory、Azure Active Directory 域服务或 IaaS 虚拟机上的 Active Directory）集成，来启用基于角色的访问控制。 群集上的域管理员可以授权用户使用他们自己的公司（域）用户名和密码来访问群集。 

    有关详细信息，请参阅：

    - [已加入域的 HDInsight 群集的 Apache Hadoop 安全性简介](./domain-joined/apache-domain-joined-introduction.md)
    - [在 HDInsight 中计划 Azure 已加入域的 Apache Hadoop 群集](./domain-joined/apache-domain-joined-architecture.md)
    - [配置已加入域的沙盒环境](./domain-joined/apache-domain-joined-configure.md)
    - [使用 Azure Active Directory 域服务配置已加入域的 HDInsight 群集](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- 数据授权

    - 与 Apache Ranger 集成以对 Hive、Spark SQL 和 Yarn 队列授权。
    - 可以对文件和文件夹设置访问控制。

    有关详细信息，请参阅：

    - [在已加入域的 HDInsight 中配置 Apache Hive 策略](./domain-joined/apache-domain-joined-run-hive.md)

- 查看审核日志以监视访问和配置的策略。 

### <a name="supported-cluster-types"></a>支持的群集类型

目前只有以下群集类型支持 Enterprise Security 包：

- Hadoop（仅限 HDInsight 3.6）
- Spark
- 交互式查询

### <a name="support-for-azure-data-lake-store"></a>支持 Azure Data Lake Store

Enterprise Security 包支持使用 Azure Data Lake Store 作为主存储和附加存储。

### <a name="pricing-and-sla"></a>定价和 SLA
有关 Enterprise Security 包的定价和 SLA 的信息，请参阅 [HDInsight 定价](https://azure.microsoft.com/pricing/details/hdinsight/)。

## <a name="hdinsight-windows-retirement"></a>HDInsight Windows 停用
Microsoft Azure HDInsight 版本 3.3 是 Windows 上的最新 HDInsight 版本。 Windows 上的 HDInsight 的停用日期是 2018 年 7 月 31 日。 如果在 Windows 上具有 HDInsight 3.3 或更低版本的群集，则必须在 2018 年 7 月 31 日之前迁移到 Linux 上的 HDInsight（HDInsight 版本 3.5 或更高版本）。 迁移到 Linux OS 后，仍然能够创建 HDInsight 群集或调整其大小。 对 Windows 上的 HDInsight 版本 3.3 的支持已于 2016 年 6 月 27 日到期。

从 HDInsight 版本 3.4 开始，Microsoft 仅在 Linux OS 上发行了 HDInsight。 因此，HDInsight 中的某些组件仅可供 Linux 使用。 这包括 [Apache Ranger](https://ranger.apache.org/)、[Apache Kafka](https://kafka.apache.org/)、交互式查询、[Apache Spark](https://spark.apache.org/)、HDInsight 应用程序以及作为主文件系统的 Azure Data Lake Store。 HDInsight 的将来版本仅在 Linux OS 上可用。 Windows 上的 HDInsight 将不会有任何将来版本。 

## <a name="faqs"></a>常见问题

### <a name="what-is-the-timeline-for-retiring-hdinsight-on-windows"></a>停用 Windows 上的 HDInsight 的日程表是怎样的？
Windows 上的 HDInsight 的停用日期是 2018 年 7 月 31 日。 如果针对你的区域计划的停用日期不同，你会单独收到通知。 

### <a name="what-is-the-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>停用 Windows 上的 HDInsight 对现有客户有何影响？
在 Windows 上的 HDInsight 停用后，将无法创建新的 HDInsight Windows 群集，也无法调整现有 HDInsight Windows 群集的大小。 对 HDInsight 版本 3.3 的支持已于 2016 年 6 月 27 日到期。 因此，不会再为 HDInsight 3.3 或更低版本提供支持或 bug 修复。 HDInsight 的将来版本仅在 Linux OS 上可用。 Windows 上的 HDInsight 将不会有任何将来版本。
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>Windows 上的哪些 HDInsight 版本会受影响？
Azure HDInsight 版本 3.3 是适用于 Windows 的最后 HDInsight 版本。 在 Windows 上的 HDInsight 停用之前，必须将所有 HDInsight Windows 3.3 或更低版本的群集迁移到 Linux 上的 HDInsight 版本 3.5 或更高版本。 将群集迁移到 Linux 上的 HDInsight 后，将仍然能够创建新群集或调整现有群集的大小。 

### <a name="what-do-i-need-to-do"></a>我需要做什么？
在 2018 年 7 月 31 日之前，将你的 HDInsight Windows 群集迁移到受支持的 HDInsight Linux 群集。 可以在 [HDInsight 迁移文档](hdinsight-migrate-from-windows-to-linux.md)中了解详细信息。 有关 Azure HDInsight 版本的详细信息，请参阅[支持的版本](hdinsight-component-versioning.md#supported-hdinsight-versions)列表。 

### <a name="where-do-i-find-the-cluster-os-type"></a>可以在哪里找到群集 OS 类型？
在 Azure 门户中，转到 HDInsight 群集概述页，然后在“概要”下找到“群集类型”。 该页上列出了群集 OS 类型。 

### <a name="i-cant-migrate-to-an-hdinsight-linux-cluster-by-july-31-2018-what-is-the-impact-to-my-hdinsight-windows-cluster"></a>我无法在 2018 年 7 月 31 日之前迁移到 HDInsight Linux 群集。 这对我的 HDInsight Windows 群集有何影响？
HDInsight Windows 群集将按现样运行，但是你将无法创建新的 HDInsight Windows 群集，也无法调整现有 HDInsight Windows 群集的大小。 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>我的群集具有 .NET 依赖项。 在 Linux 上如何解决此依赖关系？
可以使用 [Mono 项目](http://www.mono-project.com/)解决 Linux 群集依赖关系。 .NET 的此开源实现可用于 HDInsight Linux 群集。 可以在 [HDInsight 迁移文档](hdinsight-migrate-from-windows-to-linux.md)中了解详细信息。 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>我是 Windows 上的 HDInsight 的新客户。 如何创建 HDInsight Windows 群集？
从 2017 年 7 月 3 日起，只有现有 HDInsight Windows 客户可以创建新的 HDInsight Windows 群集。 新客户无法使用 PowerShell 或 SDK 在 Azure 门户中创建 HDInsight Windows 群集。 我们建议新客户创建 Linux HDInsight 群集。 现有客户仍然可以创建新的 HDInsight Windows 群集，直到 Windows 上的 HDInsight 停用的那一天。 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-to-hdinsight-on-linux"></a>从 Windows 上的 HDInsight 迁移到 Linux 上的 HDInsight 是否会有相关的定价影响？
没有，两种 OS 上的 HDInsight 的定价是相同的。 

### <a name="what-are-the-customer-advantages-associated-with-the-move-to-only-using-hdinsight-on-linux"></a>迁移后仅使用 Linux 上的 HDInsight 对客户有什么好处？
* 可以通过 HDInsight 服务加快开源大数据技术的上市时间
* 由大型社区和生态系统提供支持
* 能够利用 Hadoop 和其他大数据技术的开源社区练习主动开发

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>Linux 上的 HDInsight 是否比 Windows 上的 HDInsight 提供了更多的功能？
从 HDInsight 版本 3.4 开始，Microsoft 仅在 Linux OS 上发行了 HDInsight。 因此，HDInsight 中的某些组件仅可供 Linux 使用。 这包括 Apache Ranger、Kafka、交互式查询、Spark、HDInsight 应用程序以及作为主文件系统的 Azure Data Lake Store。 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight 群集版本的服务级别协议
服务级别协议 (SLA) 是按“支持窗口”定义的。 支持窗口是 HDInsight 群集版本受 Microsoft 客户服务和支持部门支持的时间段。 如果版本的“支持到期日期”已过，则 HDInsight 处于支持窗口外。 有关支持的版本的详细信息，请参阅[支持的 HDInsight 群集版本](hdinsight-migrate-from-windows-to-linux.md)列表。 指定的 HDInsight 版本 X 的支持到期日期（在提供更新的 X+1 版本后）为按以下公式计算所得时间的较晚者：  

* 公式 1：发布 HDInsight 群集版本 X 的日期加 180 天。
* 公式 2：在 Azure 门户中提供 HDInsight 群集版本 X+1 的日期加 90 天。

“停用日期”是指在此后不能在 HDInsight 上创建此群集版本的日期。 从 2017 年 7 月 31 日开始，无法在停用日期后调整 HDInsight 群集的大小。 

> [!NOTE]
> HDInsight Windows 群集（包括版本 2.1、3.0、3.1, 3.2 和 3.3）在 Azure 来宾 OS 系列版本 4 上运行，该版本使用 64 位版本的 Windows Server 2012 R2。 Azure 来宾 OS 系列版本 4 支持 .NET Framework 版本 4.0、4.5、4.5.1 和 4.5.2。

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>与 HDInsight 版本相关的 Hortonworks 发行说明

本部分提供了与 HDInsight 一起使用的 Hortonworks 数据平台分发版和 Apache 组件的发行说明的链接。
* HDInsight 群集版本 4.0 使用基于 [Hortonworks 数据平台 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html) 的 Hadoop 分发版
* HDInsight 群集版本 3.6 使用基于 [Hortonworks 数据平台 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html) 的 Hadoop 分发版。
* HDInsight 群集版本 3.5 使用基于 [Hortonworks 数据平台 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html) 的 Hadoop 分发版。 HDInsight 群集版本 3.5 是在 Azure 门户中创建的“默认”Hadoop 群集。
* HDInsight 群集版本 3.4 使用基于 [Hortonworks 数据平台 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) 的 Hadoop 分发版。
* HDInsight 群集版本 3.3 使用基于 [Hortonworks 数据平台 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) 的 Hadoop 分发版。

  * Apache 网站上提供了 [Apache Storm 发行说明](https://storm.apache.org/2015/11/05/storm0100-released.html)。
  * Apache 网站上提供了 [Apache Hive 发行说明](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843)。
* HDInsight 群集版本 3.2 使用基于 [Hortonworks 数据平台 2.2][hdp-2-2] 的 Hadoop 分发版。

  * 具体 Apache 组件的发行说明提供如下：[Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450)、[Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954)、[HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810)、[Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581)、[M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180)、[HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181)、[YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197)、[Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179)、[Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742)、[Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486)、[Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112) 和 [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620)。
* HDInsight 群集版本 3.1 使用基于 [Hortonworks 数据平台 2.1.7][hdp-2-1-7] 的 Hadoop 分发版。 在 2014 年 11 月 7 日之前创建的 HDInsight 3.1 群集基于 [Hortonworks 数据平台 2.1.1][hdp-2-1-1]。
* HDInsight 群集版本 3.0 使用基于 [Hortonworks 数据平台 2.0][hdp-2-0-8] 的 Hadoop 分发版。
* HDInsight 群集版本 2.1 使用基于 [Hortonworks 数据平台 1.3][hdp-1-3-0] 的 Hadoop 分发版。
* HDInsight 群集版本 1.6 使用基于 [Hortonworks 数据平台 1.1][hdp-1-1-0] 的 Hadoop 分发版。






## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>群集的默认节点配置和虚拟机大小
下表列出了 HDInsight 群集的默认虚拟机 (VM) 大小。

> [!IMPORTANT]
> 如果需要群集中有 32 个以上的辅助节点，则必须选择至少具有 8 个核心和 14 GB RAM 的头节点大小。
> 
> 

* 除巴西南部和日本西部外的所有受支持区域：

  | 群集类型 | Hadoop | HBase | 交互式查询 | Storm | Spark | ML Server |
  | --- | --- | --- | --- | --- | --- | --- |
  | 头：默认 VM 大小 |D3 v2 |D3 v2 | D13、D14 |A4 v2 |D12 v2 |D12 v2 |
  | 头：建议的 VM 大小 |D3 v2、D4 v2、D12 v2 |D3 v2、D4 v2、D12 v2  | D13、D14 |A4 v2、A8 v2、A2m v2 |D12 v2、D13 v2、D14 v2 |D12 v2、D13 v2、D14 v2 |
  | 辅助角色：默认 VM 大小 |D3 v2 |D3 v2  | D13、D14 |D3 v2 |Windows：D12 v2；Linux：D4 v2 |Windows：D12 v2；Linux：D4 v2 |
  | 辅助角色：建议的 VM 大小 |D3 v2、D4 v2、D12 v2 |D3 v2、D4 v2、D12 v2  | D13、D14 |D3 v2、D4 v2、D12 v2 |Windows：D12 v2、D13 v2、D14 v2；Linux：D4 v2、D12 v2、D13 v2、D14 v2 |Windows：D12 v2、D13 v2、D14 v2；Linux：D4 v2、D12 v2、D13 v2、D14 v2 |
  | Zookeeper：默认 VM 大小 | |A4 v2 | |A2 v2 | | |
  | Zookeeper：建议的 VM 大小 | |A4 v2、A8 v2、A2m v2 | | A2 v2、A4 v2、A8 v2 | | |
  | 边缘：默认 VM 大小 | | | | | |Windows：D12 v2；Linux：D4 v2 |
  | 边缘：建议的 VM 大小 | | | | | |Windows：D12 v2、D13 v2、D14 v2；Linux：D4 v2、D12 v2、D13 v2、D14 v2 |
* 仅限巴西南部和日本西部（无 v2 大小）：

  | 群集类型 | Hadoop | HBase | 交互式查询 |Storm | Spark | ML Services |
  | --- | --- | --- | --- | --- | --- | --- |
  | 头：默认 VM 大小 |D3 |D3  | D13、D14 |A3 |D12 |D12 |
  | 头：建议的 VM 大小 |D3、D4、D12 |D3、D4、D12  | D13、D14 |A3、A4、A5 |D12、D13、D14 |D12、D13、D14 |
  | 辅助角色：默认 VM 大小 |D3 |D3  | D13、D14 |D3 |Windows：D12；Linux：D4 |Windows：D12；Linux：D4 |
  | 辅助角色：建议的 VM 大小 |D3、D4、D12 |D3、D4、D12  | D13、D14 |D3、D4、D12 |Windows：D12、D13、D14；Linux：D4、D12、D13、D14 |Windows：D12、D13、D14；Linux：D4、D12、D13、D14 |
  | Zookeeper：默认 VM 大小 | |A2 | | A2 | | |
  | Zookeeper：建议的 VM 大小 | |A2、A3、A4 | |A2、A3、A4 | | |
  | 边缘：默认 VM 大小 | | | | | |Windows：D12；Linux：D4 |
  | 边缘：建议的 VM 大小 | | | | | |Windows：D12、D13、D14；Linux：D4、D12、D13、D14 |

> [!NOTE]
> - 对于 Storm 群集类型，头称为 *Nimbus*。
> - 对于 Storm 群集类型，辅助角色称为“主管”。
> - 对于 HBase 群集类型，辅助角色称为“区域”。

## <a name="next-steps"></a>后续步骤
- [为 HDInsight 上的 Apache Hadoop、Spark 和其他组件设置群集](hdinsight-hadoop-provision-linux-clusters.md)
- [使用 Windows 电脑在 HDInsight 上的 Apache Hadoop 中工作](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
