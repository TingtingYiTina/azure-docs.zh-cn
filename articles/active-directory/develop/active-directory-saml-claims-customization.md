---
title: 为 Azure AD 中的企业应用程序自定义 SAML 令牌中颁发的声明 | Microsoft Docs
description: 了解如何为 Azure AD 中的企业应用程序自定义 SAML 令牌中颁发的声明。
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2018
ms.author: celested
ms.reviewer: luleon, jeedes
ms.custom: aaddev
ms.openlocfilehash: afcdb7c64f4431e920f1f1fbce1e1e6d3e4db79c
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52424946"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>如何：为企业应用程序自定义 SAML 令牌中颁发的声明

目前，Azure Active Directory (Azure AD) 支持使用大多数企业应用程序进行单一登录，包括 Azure AD 应用库中预集成的两个应用程序以及自定义应用程序。 当用户通过 Azure AD 使用 SAML 2.0 协议向应用程序进行身份验证时，Azure AD 会将令牌发送到应用程序（通过 HTTP POST）。 然后，应用程序验证并使用该令牌将用户登录，而不是提示输入用户名和密码。 这些 SAML 令牌包含有关用户的信息片段（称为“声明”）。

“声明”是标识提供者在为某个用户颁发的令牌中陈述的有关该用户的信息。 在 [SAML 令牌](https://en.wikipedia.org/wiki/SAML_2.0)中，此数据通常包含在 SAML 属性语句中。 用户的唯一 ID（也称为名称标识符）通常显示在 SAML 主题中。

默认情况下，Azure AD 向应用程序颁发 SAML 令牌，其中包含 NameIdentifier 声明，以及用户在 Azure AD 中的用户名值（AKA 用户主体名称）。 此值可唯一地标识用户。 SAML 令牌还含有其他声明，其中包含用户的电子邮件地址、名字和姓氏。

若要查看或编辑 SAML 令牌中颁发给应用程序的声明，请在 Azure 门户中打开应用程序。 然后在应用程序的“用户属性”部分中，选择“查看和编辑所有其他用户属性”复选框。

![用户属性部分][1]

有两个可能的原因使你可能需要编辑 SAML 令牌中颁发的声明：
* 应用程序已编写为需要一组不同的声明 URI 或声明值。
* 应用程序的部署方式需要 NameIdentifier 声明为 Azure AD 中存储的用户名（AKA 用户主体名称）以外的其他内容。

可以编辑任何默认声明值。 在 SAML 令牌属性表中选择声明行。 这会打开“编辑属性”部分，可在其中编辑声明名称、值以及与声明关联的命名空间。

![编辑用户属性][2]

可通过单击“...”图标打开上下文菜单，然后使用该菜单删除声明（而不是 NameIdentifier）。 还可使用“添加属性”按钮添加新声明。

![编辑用户属性][3]

## <a name="editing-the-nameidentifier-claim"></a>编辑 NameIdentifier 声明

若要解决使用不同用户名部署应用程序的问题，请在“用户属性”部分的“用户标识符”下拉列表上进行选择。 此操作提供包含多个不同选项的对话框：

![编辑用户属性][4]

### <a name="attributes"></a>属性

为 `NameIdentifier`（或 NameID）声明选择所需的源。 可以从以下选项中选择。

| 名称 | Description |
|------|-------------|
| 电子邮件 | 用户的电子邮件地址 |
| userprincipalName | 用户的用户主体名称 (UPN) |
| onpremisessamaccount | 已从本地 Azure AD 同步的 SAM 帐户名 |
| objectID | Azure AD 中的用户的 objectID |
| EmployeeID | 用户的 EmployeeID |
| 目录扩展 | 目录扩展[使用 Azure AD Connect 同步从本地 Active Directory 同步](../hybrid/how-to-connect-sync-feature-directory-extensions.md) |
| 扩展属性 1-15 | 用于扩展 Azure AD 架构的本地扩展属性 |

### <a name="transformations"></a>转换

还可以使用特殊声明转换函数。

| 函数 | Description |
|----------|-------------|
| **ExtractMailPrefix()** | 从电子邮件地址、SAM 帐户名或用户主体名称中删除域后缀。 这只会提取传递用户名的第一部分（例如，“joe_smith”而不是 joe_smith@contoso.com）。 |
| **join()** | 将属性与已验证的域联接。 如果所选用户标识符值具有域，则将提取用户名以追加所选的已验证域。 例如，如果选择电子邮件 (joe_smith@contoso.com) 作为用户标识符值，并选择 contoso.onmicrosoft.com 作为已验证的域，则将生成 joe_smith@contoso.onmicrosoft.com。 |
| **ToLower()** | 将所选属性的字符转换为小写字符。 |
| **ToUpper()** | 将所选属性的字符转换为大写字符。 |

## <a name="adding-claims"></a>添加声明

添加声明时，可以指定属性名称（根据 SAML 规范，并不严格需要遵循 URI 模式）。 将值设置为目录中存储的任何用户属性。

![添加用户属性][7]

例如，需要以声明的形式发送用户在其组织中所属的部门（例如，Sales）。 输入应用程序所需的声明名称，并选择“user.department”作为值。

> [!NOTE]
> 如果指定的用户没有针对选定属性存储的值，则不会在令牌中颁发该声明。

> [!TIP]
> 仅在使用 [Azure AD Connect 工具](../hybrid/whatis-hybrid-identity.md)从本地 Active Directory 同步用户数据时，才支持“user.onpremisesecurityidentifier”和“user.onpremisesamaccountname”。

## <a name="restricted-claims"></a>受限声明

SAML 中有一些受限声明。 如果添加此类声明，Azure AD 不会发送它们。 以下是 SAML 受限声明集：

    | 声明类型 (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expired |
    | http://schemas.microsoft.com/identity/claims/accesstoken |
    | http://schemas.microsoft.com/identity/claims/openid2_id |
    | http://schemas.microsoft.com/identity/claims/identityprovider |
    | http://schemas.microsoft.com/identity/claims/objectidentifier |
    | http://schemas.microsoft.com/identity/claims/puid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier[MR1] |
    | http://schemas.microsoft.com/identity/claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groups |
    | http://schemas.microsoft.com/claims/groups.link |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/role |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/identity/claims/scope |

## <a name="next-steps"></a>后续步骤

* [Azure AD 中的应用程序管理](../manage-apps/what-is-application-management.md)
* [针对不在 Azure AD 应用程序库中的应用程序配置单一登录](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [排查基于 SAML 的单一登录的问题](howto-v1-debug-saml-sso-issues.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png
