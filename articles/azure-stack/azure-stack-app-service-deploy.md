---
title: 部署应用服务：Azure Stack | Microsoft Docs
description: 在 Azure Stack 中部署应用服务的详细指南
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: anwestg
ms.openlocfilehash: cd16bf400c5a5e5a07c7e2dc459d801e6fc810b9
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635367"
---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>将应用服务资源提供程序添加到 Azure Stack

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

根据本文中的指南在 Azure Stack 中部署应用服务。

> [!IMPORTANT]  
> 将 1809年更新应用于 Azure Stack 集成系统，或在部署 Azure 应用服务 1.4 之前部署最新 Azure Stack 开发工具包 (ASDK)。

可以让用户能够创建 Web 应用程序和 API 应用程序。 若要让用户创建这些应用程序，必须：

 - 执行本文所述步骤，将[应用服务资源提供程序](azure-stack-app-service-overview.md)添加到 Azure Stack 部署。
 - 安装应用服务资源提供程序后，可以将其包括在套餐和计划中。 然后，用户可以订阅以获取服务并开始创建应用程序。

> [!IMPORTANT]  
> 在运行资源提供程序安装程序之前，请确保已按照[准备工作](azure-stack-app-service-before-you-get-started.md)中的指南进行操作。

## <a name="run-the-app-service-resource-provider-installer"></a>运行应用服务资源提供程序安装程序

安装应用服务资源提供程序至少需要一小时。 所需时长取决于部署的角色实例数。 部署期间，安装程序运行以下任务：

 - 在指定的 Azure Stack 存储帐户中创建 blob 容器。
 - 为应用服务中创建 DNS 区域和条目。
 - 注册应用服务资源提供程序。
 - 注册应用服务库项。

若要部署应用服务资源提供程序，请执行以下步骤：

1. 在可以访问“Azure Stack 管理”Azure 资源管理终结点的计算机上，以管理员身份运行 appservice.exe。

2. 选择“部署应用服务或升级到最新版本”。

    ![应用服务安装程序][1]

3. 查看并接受 Microsoft 软件许可条款，然后选择“下一步”。

4. 查看并接受第三方许可条款，然后选择“下一步”。

5. 请确保应用服务云配置信息正确无误。 如果在 Azure Stack 开发工具包 (ASDK) 部署过程中使用了默认设置，可以接受默认值。 但是，如果在部署 ASDK 时自定义了选项，或者要部署到 Azure Stack 集成系统，则必须在此窗口中根据差异情况编辑相应的值。

   例如，如果使用域后缀 mycloud.com，则必须将“Azure Stack 租户”Azure 资源管理器终结点更改为 management.&lt;区域&gt;.mycloud.com。 查看这些设置，然后选择“下一步”以保存设置。

   ![应用服务安装程序][2]

6. 在下一“应用服务安装程序”页上，执行以下步骤：

    a. 选择“Azure Stack 订阅”旁边的“连接”。

     - 如果使用 Azure Active Directory (Azure AD)，请输入在部署 Azure Stack 时提供的 Azure AD 管理员帐户和密码。 选择“登录”。
     - 如果使用 Active Directory 联合身份验证服务 (AD FS)，请提供管理员帐户。 例如，cloudadmin@azurestack.local。 输入密码，然后选择“登录”。

   b. 在“Azure Stack 订阅”中，选择“默认提供程序订阅”。

     > [!IMPORTANT]  
     > 应用服务**必须**将其部署到**默认提供商订阅**。

   c. 在“Azure Stack 位置”中，选择要部署到的区域所对应的位置。 例如，如果要部署到 Azure Stack 开发工具包，请选择“本地”。

    ![应用服务安装程序][3]

7. 现在，可以部署到[使用这些步骤](azure-stack-app-service-before-you-get-started.md#virtual-network)配置的现有虚拟网络中，或者让应用服务安装程序创建新的虚拟网络和子网。 若要创建 VNet，请执行以下步骤：

   a. 选择“使用默认设置创建 VNet”，接受默认值，然后选择“下一步”。

   b. 也可选择“使用现有的 VNet 和子网”。 完成以下操作：

     - 选择包含虚拟网络的**资源组**。
     - 选择要部署到其中的**虚拟网络**的名称。
     - 为每个所需角色子网选择正确的“子网”值。
     - 选择“**下一步**”。

   ![应用服务安装程序][4]

8. 输入文件共享的信息，然后选择“下一步”。 文件共享的地址必须使用文件服务器的完全限定域名 (FQDN) 或 IP 地址。 例如 \\\appservicefileserver.local.cloudapp.azurestack.external\websites，或 \\\10.0.0.1\websites。

   >[!NOTE]
   >在继续下一步之前，安装程序会尝试测试与文件共享的连接。 不过，如果是部署到现有的虚拟网络，此连接测试可能会失败。 系统会发出警告，并提示你继续操作。 如果文件共享信息是正确的，请继续部署。

   ![应用服务安装程序][7]

9. 在下一“应用服务安装程序”页上，执行以下步骤：

   a. 在“标识应用程序 ID”框中，输入要用于标识的应用程序 GUID（来自 Azure AD）。

   b. 在“标识应用程序证书文件”框中，输入（或浏览到）证书文件的位置。

   c. 在“标识应用程序证书密码”框中，输入证书的密码。 此密码是在使用脚本创建证书时记下的密码。

   d. 在“Azure 资源管理器根证书文件”框中，输入（或浏览到）证书文件的位置。

   e. 选择“**下一步**”。

   ![应用服务安装程序][9]

10. 对于三个证书文件框的每一个框，请选择“浏览”并导航到相应的证书文件。 必须为每个证书提供密码。 这些证书是在[创建所需证书步骤](azure-stack-app-service-before-you-get-started.md#get-certificates)中创建的。 输入所有信息后，选择“下一步”。

    | Box | 证书文件名示例 |
    | --- | --- |
    | **应用服务默认 SSL 证书文件** | \_.appservice.local.AzureStack.external.pfx |
    | **应用服务 API SSL 证书文件** | api.appservice.local.AzureStack.external.pfx |
    | **应用服务发布者 SSL 证书文件** | ftp.appservice.local.AzureStack.external.pfx |

    如果在创建证书时使用了其他域后缀，证书文件名不要使用 *local.AzureStack.external*， 而要改用自定义域信息。

    ![应用服务安装程序][10]

11. 为用于托管应用服务资源提供程序数据库的服务器实例输入 SQL Server 详细信息，然后选择“下一步”。 安装程序将验证 SQL 连接属性。

    > [!NOTE]
    > 在继续下一步之前，安装程序会尝试测试与 SQL Server 的连接。 不过，如果是部署到现有的虚拟网络，此连接测试可能会失败。 系统会发出警告，并提示你继续操作。 如果 SQL Server 信息是正确的，请继续部署。
    >
    > 从 Azure Stack 1.3 上的 Azure 应用服务开始，安装程序将检查 SQL Server 是否在 SQL Server 级别启用了数据库包含。  如果未启用，则会出现以下异常提示：
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;  
    >    GO  
    >    RECONFIGURE;  
    >    GO
    >    ***********************************************************
    > ```
    > 有关更多详细信息，请参阅 [Azure Stack 1.3 上的 Azure 应用服务的发行说明](azure-stack-app-service-release-notes-update-three.md)。

    ![应用服务安装程序][11]

12. 查看角色实例和 SKU 选项。 默认设置中填充了 ASDK 部署中每个角色的最小实例数和最低 SKU 层级。 提供 vCPU 和内存要求摘要是为了帮助你规划部署。 进行选择后，请选择“下一步”。

    >[!NOTE]
    >对于生产部署，请按照 [Azure Stack 中 Azure 应用服务服务器角色的容量规划](azure-stack-app-service-capacity-planning.md)中的指南进行操作。

    | 角色 | 最小实例数 | 最小 SKU | 说明 |
    | --- | --- | --- | --- |
    | 控制器 | 1 | Standard_A2 -（2 个 vCPU，3584 MB） | 管理和维护应用服务云的运行状况。 |
    | 管理 | 1 | Standard_A2 -（2 vCPU，3584 MB） | 管理应用服务 Azure 资源管理器和 API 终结点、门户扩展（管理员门户、租户门户、Functions 门户）和数据服务。 为了支持故障转移，已将建议的实例数增加到 2 个。 |
    | 发布者 | 1 | Standard_A1 -（1 vCPU，1792 MB） | 通过 FTP 和 Web 部署发布内容。 |
    | FrontEnd | 1 | Standard_A1 -（1 vCPU，1792 MB） | 将请求路由到应用服务应用程序。 |
    | 共享辅助角色 | 1 | Standard_A1 -（1 vCPU，1792 MB） | 托管 Web 应用程序或 API 应用程序和 Azure Functions 应用。 可能需要添加更多实例。 作为操作员，可以定义产品/服务，并选择任何 SKU 层。 这些层必须至少具有一个 vCPU。 |

    ![应用服务安装程序][13]

    >[!NOTE]
    >**不支持将 Windows Server 2016 Core 平台映像与 Azure Stack 上的 Azure 应用服务配合使用。请勿将评估映像用于生产部署。**

13. 在“选择平台映像”框中选择 Windows Server 2016 虚拟机部署映像，该映像是应用服务云的计算资源提供程序提供的映像之一。 选择“**下一步**”。

14. 在下一“应用服务安装程序”页上，执行以下步骤：

     a. 输入辅助角色虚拟机管理员用户名和密码。

     b. 输入其他角色虚拟机管理员用户名和密码。

     c. 选择“**下一步**”。

    ![应用服务安装程序][15]

15. 在“应用服务安装程序”摘要页上，执行以下步骤：

    a. 验证所做的选择。 若要进行更改，请使用“上一步”按钮访问前面的页面。

    b. 如果配置正确，则选中此复选框。

    c. 若要开始部署，请选择“下一步”。

    ![应用服务安装程序][16]

16. 在下一“应用服务安装程序”页上，执行以下步骤：

    a. 跟踪安装进度。 Azure Stack 上的应用服务大约需要 60 分钟才能完成基于默认选择的部署。

    b. 安装程序成功完成后，请选择“退出”。

    ![应用服务安装程序][17]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>验证 Azure Stack 上的应用服务安装

1. 在 Azure Stack 管理员门户中，转到“管理 - 应用服务”。

2. 在概述中，在状态检查以查看是否**状态**显示**所有角色都已就绪**。

    ![应用服务管理](media/azure-stack-app-service-deploy/image12.png)

    如果部署到现有虚拟网络并使用内部 IP 地址连接到文件服务器，则必须添加出站安全规则。 此规则允许辅助角色子网和文件服务器之间的 SMB 流量。  为此，请转到管理门户中的 WorkersNsg 并添加具有以下属性的出站安全规则：

    - 源：任何
    - 源端口范围：*
    - 目标：IP 地址
    - 目标 IP 地址范围：文件服务器的 IP 范围
    - 目标端口范围：445
    - 协议：TCP
    - 操作：允许
    - 优先级：700
    - 名称：Outbound_Allow_SMB445

## <a name="test-drive-app-service-on-azure-stack"></a>体验 Azure Stack 上的应用服务

部署并注册应用服务资源提供程序后，对其进行测试以确保用户可以部署 Web 应用和 API 应用。

>[!NOTE]
>需要创建一个套餐，其中的计划包含 Microsoft.Web 命名空间。 此外还需订阅此套餐的租户订阅。 有关详细信息，请参阅[创建套餐](azure-stack-create-offer.md)和[创建计划](azure-stack-create-plan.md)。
>
>必须有租户订阅，才能创建使用 Azure Stack 上的应用服务的应用程序。 服务管理员只能在管理员门户中完成的任务与资源提供程序对应用服务的管理相关。 这包括添加容量、配置部署源以及添加辅助角色层和 SKU。
>
>若要创建 Web 应用、API 应用和 Azure Functions 应用，必须使用租户门户并具有租户订阅。
>

若要创建测试性 Web 应用，请执行以下步骤：

1. 在 Azure Stack 用户门户中，选择“+ 创建资源” > “Web + 移动” > “Web 应用”。

2. 在“Web 应用”下的“Web 应用”中输入一个名称。

3. 在“资源组”下，选择“新建”。 输入**资源组**的名称。

4. 选择“应用服务计划/位置” > “新建”。

5. 在“应用服务计划”下，输入**应用服务计划**的名称。

6. 选择“定价层” > “免费共享”或“共享共享” > “选择” > “确定” > “创建”。

7. 此时新 Web 应用的磁贴会显示在仪表板上。 选择磁贴。

8. 在“Web 应用”上选择“浏览”，查看此应用的默认网站。

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>部署 WordPress、DNN 或 Django 网站（可选）

1. 在 Azure Stack 租户门户中选择“+”，转到 Azure 市场，部署 Django 网站，然后等待部署完成。 Django Web 平台使用基于文件系统的数据库。 它不需要任何其他资源提供程序，如 SQL 或 MySQL。

2. 如果还部署了 MySQL 资源提供程序，则可从市场部署 WordPress 网站。 当系统提示输入数据库参数时，请输入用户名，其格式为 *User1@Server1*（使用所选的用户名和服务器名称）。

3. 如果还部署了 SQL Server 资源提供程序，则可从市场部署 DNN 网站。 当系统提示输入数据库参数时，请在运行 SQL Server 的计算机中选择连接到资源提供程序的数据库。

## <a name="next-steps"></a>后续步骤

还可以试用其他[平台即服务 (PaaS) 服务](azure-stack-tools-paas-services.md)。

 - [SQL Server 资源提供程序](azure-stack-sql-resource-provider-deploy.md)
 - [MySQL 资源提供程序](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy/app-service-installer.png
[2]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-arm-endpoints.png
[3]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-subscription-information.png
[4]: ./media/azure-stack-app-service-deploy/app-service-default-VNET-config.png
[5]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config-with-values.png
[7]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration.png
[8]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration-error.png
[9]: ./media/azure-stack-app-service-deploy/app-service-identity-app.png
[10]: ./media/azure-stack-app-service-deploy/app-service-certificates.png
[11]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration.png
[12]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration-error.png
[13]: ./media/azure-stack-app-service-deploy/app-service-cloud-quantities.png
[14]: ./media/azure-stack-app-service-deploy/app-service-windows-image-selection.png
[15]: ./media/azure-stack-app-service-deploy/app-service-role-credentials.png
[16]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-deployment-summary.png
[17]: ./media/azure-stack-app-service-deploy/app-service-deployment-progress.png
