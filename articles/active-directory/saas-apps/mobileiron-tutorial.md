---
title: 教程：Azure Active Directory 与 MobileIron 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 MobileIron 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3e4bbd5b-290e-4951-971b-ec0c1c11aaa2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2017
ms.author: jeedes
ms.openlocfilehash: 1b6527207793558c132be4cf004b7d6fdde14a90
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584105"
---
# <a name="tutorial-azure-active-directory-integration-with-mobileiron"></a>教程：Azure Active Directory 与 MobileIron 集成

在本教程中，了解如何将 MobileIron 与 Azure Active Directory (Azure AD) 集成。

将 MobileIron 与 Azure AD 集成提供以下优势：

- 可以在 Azure AD 中控制谁有权访问 MobileIron
- 可以让用户使用其 Azure AD 帐户自动登录到 MobileIron（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 MobileIron 的集成，需要以下项：

- Azure AD 订阅
- 启用了 MobileIron 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库添加 MobileIron
1. 配置和测试 Azure AD 单一登录

## <a name="adding-mobileiron-from-the-gallery"></a>从库添加 MobileIron
要配置 MobileIron 与 Azure AD 的集成，需要从库中将 MobileIron 添加到托管 SaaS 应用列表。

**若要从库中添加 MobileIron，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“MobileIron”，在结果面板中选择“MobileIron”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 MobileIron](./media/mobileiron-tutorial/tutorial_mobileiron_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于一个名为“Britta Simon”的测试用户使用 MobileIron 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 MobileIron 用户。 换句话说，需要建立 Azure AD 用户与 MobileIron 中相关用户之间的链接关系。

通过将 Azure AD 中“用户名”的值指定为 MobileIron 中“用户名”的值来建立此链接关系。

若要配置和测试 MobileIron 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 MobileIron 测试用户](#create-a-mobileiron-test-user)** - 在 MobileIron 中创建 Britta Simon 的对应用户，并将其链接到用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将介绍如何在 Azure 门户中启用 Azure AD 单一登录并在 MobileIron 应用程序中配置单一登录。

**若要配置 MobileIron 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 **MobileIron** 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/mobileiron-tutorial/tutorial_mobileiron_samlbase.png)

1. 若要在“IDP” 发起的模式下配置应用程序，请在“MobileIron 域和 URL”部分中，执行以下步骤 ****：

    ![MobileIron 域和 URL 单一登录信息](./media/mobileiron-tutorial/tutorial_mobileiron_url.png)

    1. 在“标识符”文本框中，使用以下模式键入 URL：`https://www.mobileiron.com/<key>`

    1. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://<host>.mobileiron.com/saml/SSO/alias/<key>`

1. 若要在“SP” 发起的模式下配置应用程序，请选中“显示高级 URL 设置”  **** ****，并执行以下步骤：

    ![MobileIron 域和 URL 单一登录](./media/mobileiron-tutorial/tutorial_mobileiron_url1.png)

    在“登录 URL” **** 文本框中，使用以下模式键入 URL： `https://<host>.mobileiron.com/user/login.html`
    
    > [!NOTE]  这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 可从 MobileIron 的管理门户中获取该密钥和主机的值，本教程稍后会做介绍。

1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![证书下载链接](./media/mobileiron-tutorial/tutorial_mobileiron_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/mobileiron-tutorial/tutorial_general_400.png)

1. 在另一个 Web 浏览器窗口中，以管理员身份登录 MobileIron 公司站点。

1. 转到“管理” > “标识”。

   * 在“有关云 IDP 设置的信息”字段中选择“AAD”选项。

    ![配置单一登录管理按钮](./media/mobileiron-tutorial/tutorial_mobileiron_admin.png)

1. 复制并粘贴“密钥”和“主机”的值，在 Azure 门户的“MobileIron 域和 URL”部分中填写 URL。

    ![配置单一登录管理按钮](./media/mobileiron-tutorial/key.png)

1. 在“从 AAD 导出元数据文件并导入到 MobileIron 云字段”中，单击“选择文件”从 Azure 门户上传已下载的元数据。 上传完成后，单击“完成”。
 
    ![配置单一登录管理元数据按钮](./media/mobileiron-tutorial/tutorial_mobileiron_adminmetadata.png)

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[ Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/mobileiron-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/mobileiron-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/mobileiron-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/mobileiron-tutorial/create_aaduser_04.png)

    1. 在“姓名”框中，键入“BrittaSimon”。

    1. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    1. 单击“创建”。
  
### <a name="create-a-mobileiron-test-user"></a>创建 MobileIron 测试用户

为了使 Azure AD 用户能够登录到 MobileIron，必须将其预配到 MobileIron 中。  
就 MobileIron 来说，预配任务需要手动完成。

**若要预配用户帐户，请执行以下步骤：**

1. 以管理员身份登录到 MobileIron 公司站点。

1. 转到“用户”，单击“添加” > “单一用户”。

    ![配置单一登录用户按钮](./media/mobileiron-tutorial/tutorial_mobileiron_user.png)

1. 在“单个用户”对话框页上，执行以下步骤：

    ![配置单一登录用户添加按钮](./media/mobileiron-tutorial/tutorial_mobileiron_useradd.png)

    1. 在“电子邮件地址”文本框中，输入用户的电子邮件地址，例如 brittasimon@contoso.com。

    1. 在“名字”文本框中，输入用户的名字，例如 Britta。

    1. 在“姓氏”文本框中，输入用户的姓氏，例如 Simon。
    
    1. 单击“完成”。  

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 MobileIron 的权限，允许她使用 Azure 单一登录。

![分配用户角色][200] 

**若要将 Britta Simon 分配到 MobileIron，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“MobileIron”。

    ![应用程序列表中的 MobileIron 链接](./media/mobileiron-tutorial/tutorial_mobileiron_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

当在访问面板中单击 MobileIron 磁贴时，应会自动登录到 MobileIron 应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](../user-help/active-directory-saas-access-panel-introduction.md)。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/mobileiron-tutorial/tutorial_general_01.png
[2]: ./media/mobileiron-tutorial/tutorial_general_02.png
[3]: ./media/mobileiron-tutorial/tutorial_general_03.png
[4]: ./media/mobileiron-tutorial/tutorial_general_04.png

[100]: ./media/mobileiron-tutorial/tutorial_general_100.png

[200]: ./media/mobileiron-tutorial/tutorial_general_200.png
[201]: ./media/mobileiron-tutorial/tutorial_general_201.png
[202]: ./media/mobileiron-tutorial/tutorial_general_202.png
[203]: ./media/mobileiron-tutorial/tutorial_general_203.png

