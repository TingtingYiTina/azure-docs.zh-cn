---
title: 教程：使用 IntelliJ 在 Azure HDInsight 中创建适用于 Spark 的 Scala Maven 应用程序
description: 使用 Apache Maven 作为生成系统，并使用 IntelliJ IDEA 提供的适用于 Scala 的现有 Maven 原型，创建使用 Scala 编写的 Spark 应用程序。
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/07/2018
ms.openlocfilehash: d83c04946b67dd25bae306c2fa41a0864287bfc8
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52499310"
---
# <a name="tutorial-create-a-scala-maven-application-for-apache-spark-in-hdinsight-using-intellij"></a>教程：使用 IntelliJ 在 HDInsight 中创建适用于 Apache Spark 的 Scala Maven 应用程序

本教程介绍如何结合使用 [Apache Maven](https://maven.apache.org/) 和 IntelliJ IDEA 创建用 [Scala](https://www.scala-lang.org/) 编写的 [Apache Spark](https://spark.apache.org/) 应用程序。 本文将 Apache Maven 用作生成系统，并从 IntelliJ IDEA 提供的 Scala 的现有 Maven 原型入手。  在 IntelliJ IDEA 中创建 Scala 应用程序需要以下步骤：

* 将 Maven 用作生成系统。
* 更新项目对象模型 (POM) 文件，以解析 Spark 模块依赖项。
* 使用 Scala 编写应用程序。
* 生成可以提交到 HDInsight Spark 群集的 jar 文件。
* 使用 Livy 在 Spark 群集上运行应用程序。

> [!NOTE]
> HDInsight 还提供了一个 IntelliJ IDEA 插件工具，用于简化在 Linux 上创建应用程序并提交到 HDInsight Spark 群集的过程。 有关详细信息，请参阅[使用适用于 IntelliJ IDEA 的 HDInsight 工具插件以创建和提交 Apache Spark 应用程序](apache-spark-intellij-tool-plugin.md)。
> 

本教程介绍如何执行下列操作：
> [!div class="checklist"]
> * 使用 IntelliJ 开发 Scala Maven 应用程序

如果还没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。


## <a name="prerequisites"></a>先决条件

* HDInsight 上的 Apache Spark 群集。 有关说明，请参阅[在 Azure HDInsight 中创建 Apache Spark 群集](apache-spark-jupyter-spark-sql.md)。
* Oracle Java 开发工具包。 可以从[此处](https://aka.ms/azure-jdks)安装。
* Java IDE。 本文使用 IntelliJ IDEA 18.1.1。 可从[此处](https://www.jetbrains.com/idea/download/)进行安装。

## <a name="use-intellij-to-create-application"></a>使用 IntelliJ 创建应用程序

1. 启动 IntelliJ IDEA 并创建一个项目。 在“新建项目”对话框中执行以下操作： 

   a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 选择“HDInsight” > “Spark on HDInsight (Scala)”

   b. 在“生成工具”列表中，根据需要选择以下选项之一：

      * 用于支持 Scala 项目创建向导的“Maven”
      * 用于管理依赖项和生成 Scala 项目的“SBT”

   ![“新建项目”对话框](./media/apache-spark-create-standalone-application/create-hdi-scala-app.png)

1. 选择“**下一步**”。

1. Scala 项目创建向导会自动检测是否安装了 Scala 插件。 选择“安装”。

   ![Scala 插件检查](./media/apache-spark-create-standalone-application/Scala-Plugin-check-Reminder.PNG) 

1. 若要下载 Scala 插件，请选择“确定”。 按照说明重新启动 IntelliJ。 

   ![Scala 插件安装对话框](./media/apache-spark-create-standalone-application/Choose-Scala-Plugin.PNG)

1. 在“新建项目”窗口中执行以下操作：  

    ![选择 Spark SDK](./media/apache-spark-create-standalone-application/hdi-new-project.png)

   a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 输入项目名称和位置。

   b. 在“项目 SDK”下拉列表中，选择适用于 Spark 2.x 群集的“Java 1.8”，或选择适用于 Spark 1.x 群集的“Java 1.7”。

   c. 在“Spark 版本”下拉列表中，Scala 项目创建向导集成了 Spark SDK 和 Scala SDK 的适当版本。 如果 Spark 群集版本低于 2.0，请选择“Spark 1.x”。 否则，请选择“Spark 2.x”。 本示例使用“Spark 2.0.2 (Scala 2.11.8)”。

1. 选择“完成”。

## <a name="install-scala-plugin-for-intellij-idea"></a>安装适用于 IntelliJ IDEA 的 Scala 插件
若要安装 Scala 插件，请使用以下步骤：

1. 打开 IntelliJ IDEA。
1. 在**欢迎**屏幕上，依次选择配置、**插件**。
   
    ![启用 scala 插件](./media/apache-spark-create-standalone-application/enable-scala-plugin.png)
1. 选择左下角的“安装 JetBrains 插件”。 
1. 在“浏览 JetBrains 插件”对话框中搜索 Scala，并选择“安装”。
   
    ![安装 scala 插件](./media/apache-spark-create-standalone-application/install-scala-plugin.png)
1. 成功安装该插件后，必须重启 IDE。

## <a name="create-a-standalone-scala-project"></a>创建独立 Scala 项目
1. 打开 IntelliJ IDEA。
1. 从“文件”菜单中选择“新建”>“项目”，创建新的项目。
1. 在“新建项目”对话框中做出以下选择：
   
    ![创建 Maven 项目](./media/apache-spark-create-standalone-application/create-maven-project.png)
   
   * 选择“Maven”作为项目类型。
   * 指定“项目 SDK”。 选择“新建”并导航到 Java 安装目录，通常是 `C:\Program Files\Java\jdk1.8.0_66`。
   * 选择“从原型创建”选项。
   * 从原型列表中，选择“org.scala-tools.archetypes:scala-archetype-simple”。 此原型会创建适当的目录结构，并下载所需的默认依赖项来编写 Scala 程序。
1. 选择“**下一步**”。
1. 提供 **GroupId**、**ArtifactId** 和 **Version** 的相关值。 本教程涉及以下值：

    - GroupId: com.microsoft.spark.example
    - ArtifactId: SparkSimpleApp
1. 选择“**下一步**”。
1. 验证设置，并选择“下一步”。
1. 验证项目名称和位置，并选择“完成”。
1. 在左侧窗格中，选择“src”>“测试”>“scala”>“com”>“microsoft”>“spark”>“示例”，右键单击“MySpec”，并选择“删除”。 应用程序不需要此文件。
  
1. 后续步骤会更新 pom.xml 以定义 Spark Scala 应用程序的依赖项。 要实现这些依赖项的自动下载和解析，必须对 Maven 进行相应配置。
   
    ![配置 Maven 以进行自动下载](./media/apache-spark-create-standalone-application/configure-maven.png)
   
   1. 从“文件”菜单中，选择“设置”。
   1. 在“设置”对话框中，导航到“生成、执行、部署” > “生成工具” > “Maven” > “导入”。
   1. 选择“自动导入 Maven 项目”选项。
   1. 依次选择“应用”、“确定”。
1. 在左侧窗格中，选择“src”>“main”>“scala”>“com.microsoft.spark.example”，然后双击“应用”以打开 App.scala。

1. 将当前示例代码替换为以下代码，然后保存所做的更改。 此代码从 HVAC.csv（所有 HDInsight Spark 群集均有该文件）中读取数据，检索第六列中只有一个数字的行，并将输出写入群集的默认存储容器下的 **/HVACOut**。

        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
1. 在左侧窗格中，双击“pom.xml”。
   
   1. 在 `<project>\<properties>` 中添加以下段：
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   1. 在 `<project>\<dependencies>` 中添加以下段：
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      将更改保存到 pom.xml。
1. 创建 .jar 文件。 IntelliJ IDEA 允许创建 JAR，将其作为项目的一项。 执行以下步骤。
    
    1. 在“文件”菜单中，选择“项目结构”。
    1. 在“项目结构”对话框中，选择“项目”，并选择加号。 在弹出的对话框中，选择“JAR”，并选择“从包含依赖项的模块”。
       
        ![创建 JAR](./media/apache-spark-create-standalone-application/create-jar-1.png)
    1. 在“从模块创建 JAR”对话框中，选择“主类”旁边的省略号 (![ellipsis](./media/apache-spark-create-standalone-application/ellipsis.png))。
    1. 在“选择主类”对话框中，选择默认显示的类，并选择“确定”。
       
        ![创建 JAR](./media/apache-spark-create-standalone-application/create-jar-2.png)
    1. 在“从模块创建 JAR”对话框中，确保已选择“提取到目标 JAR”选项，并选择“确定”。  这设置会创建包含所有依赖项的单个 JAR。
       
        ![创建 JAR](./media/apache-spark-create-standalone-application/create-jar-3.png)
    1. “输出布局”选项卡列出了所有包含为 Maven 项目一部分的 jar。 可以选择并删除 Scala 应用程序不直接依赖的 jar。 对于此处创建的应用程序，可以删除最后一个（SparkSimpleApp 编译输出）以外的所有 jar。 选择要删除的 jar，并选择“删除”图标。
       
        ![创建 JAR](./media/apache-spark-create-standalone-application/delete-output-jars.png)
       
        请务必选中“包含在项目生成中”框，以确保每次生成或更新项目时都创建 jar。 选择“应用”，并选择“确定”。
    1. 从“生成”菜单中，选择“生成项目”以创建 jar。 输出 jar 会在 **\out\artifacts** 下创建。
       
        ![创建 JAR](./media/apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-apache-spark-cluster"></a>在 Apache Spark 群集上运行应用程序
若要在群集上运行应用程序，可以使用以下方法：

* **将应用程序 jar 复制到群集关联的 Azure 存储 blob**。 可以使用命令行实用工具 [**AzCopy**](../../storage/common/storage-use-azcopy.md) 来执行此操作。 也可以使用许多其他客户端来上传数据。 有关详细信息，请参阅[在 HDInsight 中上传 Apache Hadoop 作业的数据](../hdinsight-upload-data.md)。
* **使用 Apache Livy 将应用程序作业远程提交**到 Spark 群集。 HDInsight 上的 Spark 群集包括公开 REST 终结点的 Livy，可远程提交 Spark 作业。 有关详细信息，请参阅[将 Apache Livy 与 HDInsight 上的 Apache Spark 群集配合使用以远程提交 Spark 作业](apache-spark-livy-rest-interface.md)。

## <a name="next-step"></a>后续步骤

本文介绍了如何创建 Apache Spark scala 应用程序。 转到下一文章，了解如何使用 Livy 在 HDInsight Spark 群集上运行此应用程序。

> [!div class="nextstepaction"]
>[使用 Apache Livy 在 Apache Spark 群集中远程运行作业](./apache-spark-livy-rest-interface.md)

