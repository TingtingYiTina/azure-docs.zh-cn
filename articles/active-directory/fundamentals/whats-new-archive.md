---
title: Azure Active Directory 的新增功能存档 | Microsoft Docs
description: 此内容集的“概述”部分中的新增功能发行说明包含 6 个月的活动。 6 个月过后，这些项目将从主文章中删除并放入此存档文章中。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: 54e63cf90d72b64dbe00ab8b65179f015c23b646
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427601"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Azure Active Directory 的新增功能存档

主要的[新增功能发行说明](whats-new.md)文章包含最近 6 个月的信息，而本文包含所有旧信息。

新增功能发行说明提供以下相关信息：

- 最新版本
- 已知问题
- Bug 修复
- 已弃用的功能
- 更改计划

---
 
## <a name="april-2018"></a>2018 年 4 月 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C 访问令牌已发布正式版

**类型：** 新功能  
**服务类别：** B2C - 用户标识管理  
**产品功能：** B2B/B2C 

现在可以使用访问令牌访问受 Azure AD B2C 保护的 Web API。 该功能将从公共预览版过渡到正式版。 改进了配置 Azure AD B2C 应用程序和 Web API 的 UI 体验，并进行了其他微小改进。
 
有关详细信息，请参阅 [Azure AD B2C：请求访问令牌](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens)。

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>测试基于 SAML 的应用程序的单一登录配置

**类型：** 新功能  
**服务类别：** 企业应用  
**产品功能：** SSO

配置基于 SAML 的 SSO 应用程序时，能够在配置页上测试集成。 如果在登录期间遇到错误，可以在测试体验中提供错误，Azure AD 将提供用于解决特定问题的解决方法步骤。

有关详细信息，请参阅：

- [针对不在 Azure Active Directory 应用程序库中的应用程序配置单一登录](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [如何在 Azure Active Directory 中调试对应用程序进行基于 SAML 的单一登录](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD 使用条款现在具有每个用户的报告

**类型：** 新功能  
**服务类别：** 使用条款  
**产品功能：** 符合性
 
管理员现在可以选择给定 ToU，查看已同意该 ToU 的所有用户以及同意发生的日期/时间。

有关详细信息，请参阅 [Azure AD 使用条款功能](https://docs.microsoft.com/azure/active-directory/active-directory-tou)。

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health：AD FS Extranet 锁定保护的风险 IP 

**类型：** 新功能  
**服务类别：** 其他  
**产品功能：** 监视和报告

Connect Health 现在支持按小时或按天检测超过失败 U/P 登录次数阈值的 IP 地址的功能。 该功能提供的功能包括：

- 使用可自定义阈值按小时/按天生成的综合报告，显示失败登录的 IP 地址和次数。
- 基于电子邮件的警报，当特定 IP 地址按小时/按天超出了失败 U/P 登录次数阈值时显示。
- 用于对数据进行详细分析的下载选项

有关详细信息，请参阅[风险 IP 报表](https://aka.ms/aadchriskyip)。

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>包含元数据文件或 URL 的简单应用配置

**类型：** 新功能  
**服务类别：** 企业应用  
**产品功能：** SSO

在“企业应用程序”页上，管理员可以上传 SAML 元数据文件，以便为 AAD 库和非库应用程序配置基于 SAML 的登录。

此外，还可以使用 Azure AD 应用程序联合元数据 URL 为目标应用程序配置 SSO。

有关详细信息，请参阅[针对不在 Azure Active Directory 应用程序库中的应用程序配置单一登录](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)。

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD 使用条款现已正式发布

**类型：** 新功能  
**服务类别：** 使用条款  
**产品功能：** 符合性
 

Azure AD 使用条款已从公共预览版过渡到正式版。

有关详细信息，请参阅 [Azure AD 使用条款功能](https://docs.microsoft.com/azure/active-directory/active-directory-tou)。

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>允许或阻止向特定组织中的 B2B 用户发送邀请

**类型：** 新功能  
**服务类别：** B2B  
**产品功能：** B2B/B2C
 

现在可以在 Azure AD B2B 协作中指定要与之共享和协作的合作伙伴组织。 为此，可以选择创建具体允许或拒绝域的列表。 使用这些功能阻止某个域时，员工可以不再向该域中的人员发送邀请。

这可帮助你控制对资源的访问，同时可使获得批准的用户的体验更流畅。

此 B2B 协作功能可供所有 Azure Active Directory 客户使用，并可与 Azure AD Premium 功能（如条件访问和标识保护）结合使用，以更精细地控制外部业务用户注册和获得访问权限的时间和方式。

有关详细信息，请参阅[允许或阻止向特定组织中的 B2B 用户发送邀请](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list)。

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD 应用库中提供了新的联合应用

**类型：** 新功能  
**服务类别：** 企业应用  
**产品功能：** 第三方集成

我们已于 2018 年 4 月将这 13 款支持联合的新应用添加到了我们的应用库中：

Criterion HCM、[FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial)、[Secret Server (On-Premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial)、[Dynamic Signal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial)、[mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial)、[OrgChart Now](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial)、[Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial)、[AppNeta Performance Monitor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial)、[Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial)、[Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial)、[Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial)、Shelf、[SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

有关这些应用的详细信息，请参阅 [SaaS 应用程序与 Azure Active Directory 集成](https://aka.ms/appstutorial)。

要详细了解如何在 Azure AD 应用库中列出应用程序，请参阅[在 Azure Active Directory 应用程序库中列出应用程序](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>向 Azure AD 中 B2B 用户授予对本地应用程序（公共预览版）的访问权限

**类型：** 新功能  
**服务类别：** B2B  
**产品功能：** B2B/B2C

使用 Azure Active Directory (Azure AD) B2B 协作功能邀请合作伙伴组织中的来宾用户加入 Azure AD 的组织现在可以向这些 B2B 用户提供本地应用的访问权限。 这些本地应用可以结合 Kerberos 约束委派 (KCD) 使用基于 SAML 的身份验证或 Windows 集成身份验证 (IWA)。

有关详细信息，请参阅[向 Azure AD 中的 B2B 用户授予对本地应用程序的访问权限](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises)。

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>从 Azure 市场获取 SSO 集成教程

**类型：** 已更改的功能  
**服务类别：** 其他  
**产品功能：** 第三方集成

如果 [Azure 市场](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1)中列出的应用程序支持基于 SAML 的单一登录，则单击“立即获取”会为你提供与该应用程序关联的集成教程。 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>加快 Azure AD 对 SaaS 应用程序的自动用户预配性能

**类型：** 已更改的功能  
**服务类别：** 应用预配  
**产品功能：** 第三方集成
 
以前，在以下情况下，将 Azure Active Directory 用户预配连接器用于 SaaS 应用程序（例如，Salesforce、ServiceNow 和 Box）的客户可能会遇到性能缓慢的情况：其 Azure AD 租户包含的组合用户和组超过 100,000 个，并且他们使用用户和组分配来确定应预配哪些用户。

在 2018 年 4 月 2 日，非常显著的性能增强功能已部署到 Azure AD 预配服务，以大幅减少执行 Azure Active Directory 和目标 SaaS 应用程序之间的初始同步所需的时间。

因此，与应用的初始同步需要多天或从未完成的许多客户现在可以在大约几分钟或几小时内完成。

有关详细信息，请参阅[预配期间会发生什么情况？](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>从已加入混合 Azure AD 的计算机的 Windows 10 锁定屏幕进行自助服务密码重置

**类型：** 已更改的功能  
**服务类别：** 自助服务密码重置  
**产品功能：** 用户身份验证
 
我们已更新 Windows 10 SSPR 功能，以支持已加入混合 Azure AD 的计算机。 此功能在 Windows 10 RS4 中提供，允许用户从 Windows 10 计算机的锁屏界面重置其密码。 已启用并已注册自助服务密码重置的用户可以利用此功能。

有关详细信息，请参阅[从登录屏幕进行 Azure AD 密码重置](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows)。

---

## <a name="march-2018"></a>2018 年 3 月
 
### <a name="certificate-expire-notification"></a>证书过期通知

**类型：** 已修复  
**服务类别：** 企业应用  
**产品功能：** SSO
 
当库或非库应用程序的证书即将过期时，Azure AD 会发送通知。 

对于配置了基于 SAML 的单一登录的企业应用程序，某些用户无法收到通知。 此问题已解决。 如果证书在 7、30 和 60 天内过期，Azure AD 会发送通知。 可以在审核日志中查看此事件。 

有关详细信息，请参阅：

- [在 Azure Active Directory 中管理用于联合单一登录的证书](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory 门户中的“审核活动”报表](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C 中的 Twitter 和 GitHub 标识提供者

**类型：** 新功能  
**服务类别：** B2C - 用户标识管理  
**产品功能：** B2B/B2C
 
现在能以标识提供者的身份在 Azure AD B2C 中添加 Twitter 或 GitHub。 Twitter 将从公共预览版过渡到正式版。 GitHub 即将发布公共预览版。

有关详细信息，请参阅[什么是 Azure AD B2B 协作？](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>将 Intune Managed Browser 与基于 Azure AD 应用程序的条件访问配合使用限制 iOS 和 Android 的浏览器访问

**类型：** 新功能  
**服务类别：** 条件访问  
**产品功能：** 标识安全和保护
 
**现在处于公开预览状态！**

**Intune Managed Browser SSO：** 员工可以将跨本机客户端（如 Microsoft Outlook）的单一登录和 Intune Managed Browser 用于 Azure AD 连接的所有应用。

**Intune Managed Browser 条件访问支持：** 你现在可以要求员工通过基于应用程序的条件访问策略使用 Intune Managed browser。

有关详细信息，请阅读我们的[博客文章](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/)。

有关详细信息，请参阅：

- [设置基于应用程序的条件访问](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [配置 Managed browser 策略](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>Powershell GA 模块中的应用代理 Cmdlet

**类型：** 新功能  
**服务类别：** 应用代理  
**产品功能：** 访问控制
 
Powershell GA 模块现已提供应用程序代理 cmdlet 的支持！ 这需要随时更新 Powershell 模块 - 如果超过一年未更新，某些 cmdlet 可能会停止工作。 

有关详细信息，请参阅 [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0)。
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>使用非交互式协议的无缝 SSO 支持 Office 365 本机客户端

**类型：** 新功能  
**服务类别：** 身份验证（登录）  
**产品功能：** 用户身份验证
 
使用 Office 365 本机客户端（16.0.8730.xxxx 和更高版本）的用户在使用无缝 SSO 时会获得静默登录体验。 这项支持是通过在 Azure AD 中添加非交互式协议 (WS-Trust) 提供的。

有关详细信息，请参阅[如何使用无缝 SSO 在本地客户端上进行登录？](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>如果应用程序将登录请求发送到 Azure AD 的租用终结点，则用户在使用无缝 SSO 时会获得静默登录体验

**类型：** 新功能  
**服务类别：** 身份验证（登录）  
**产品功能：** 用户身份验证
 
如果应用程序（例如 `https://contoso.sharepoint.com`）向 Azure AD 的租户终结点（即 `https://login.microsoftonline.com/contoso.com/<..>` 或 `https://login.microsoftonline.com/<tenant_ID>/<..>`）而不是 Azure AD 的普通终结点 (`https://login.microsoftonline.com/common/<...>`) 发送登录请求，用户在使用无缝 SSO 时可以获得静默登录体验。

有关详细信息，请参阅 [Azure Active Directory 无缝单一登录](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)。 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>只需将一个 Azure AD URL（以前为两个 URL）添加到用户的 Intranet 区域设置，即可实施无缝 SSO

**类型：** 新功能  
**服务类别：** 身份验证（登录）  
**产品功能：** 用户身份验证
 
若要向用户实施无缝 SSO，只需使用 Active Directory 中的组策略将一个 Azure AD URL 添加到用户的 Intranet 区域设置：`https://autologon.microsoftazuread-sso.com`。 以前，客户需要添加两个 URL。

有关详细信息，请参阅 [Azure Active Directory 无缝单一登录](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)。 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD 应用库中提供了新的联合应用

**类型：** 新功能  
**服务类别：** 企业应用  
**产品功能：** 第三方集成

我们已于 2018 年 3 月将这 15 款支持联合的新应用添加到了我们的应用库中：

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial)、[CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial)、Wrike、[SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial)、Assistant by FirstAgenda、[YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial)、Vtiger CRM、inwink、[Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial)、[Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial)、[ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial)、[Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial)、[Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial)、[Trisotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial)、[Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial)。
 
有关这些应用的详细信息，请参阅 [SaaS 应用程序与 Azure Active Directory 集成](https://aka.ms/appstutorial)。

要详细了解如何在 Azure AD 应用库中列出应用程序，请参阅[在 Azure Active Directory 应用程序库中列出应用程序](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>Azure 资源的 PIM 已发布正式版

**类型：** 新功能  
**服务类别：** Privileged Identity Management  
**产品功能：** Privileged Identity Management
 
如果将 Azure AD Privileged Identity Management 用于目录角色，现在可将 PIM 的时限访问和分配功能用于 Azure 资源角色，例如订阅、资源组、虚拟机和 Azure 资源管理器支持的其他任何资源。 实时激活角色时强制实施多重身份验证，并根据批准的更改时间范围计划激活。 此外，此版本添加了公共预览版中未提供的增强功能，包括更新的 UI、审批工作流，并且能够延长即将过期的角色，以及续订过期的角色。

有关详细信息，请参阅 [Azure 资源的 PIM（预览）](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>将可选声明添加到应用令牌（公共预览版）

**类型：** 新功能  
**服务类别：** 身份验证（登录）  
**产品功能：** 用户身份验证
 
Azure AD 应用现在可以在 JWT 或 SAML 令牌中请求自定义或可选声明。  因为大小或适用性方面的约束，这些有关用户或租户的声明默认不会包含在令牌中。  此功能目前已在 v1.0 和 v2.0 终结点上的 Azure AD 应用公共预览版中提供。  请参阅文档，以了解可添加的声明，以及如何编辑应用程序清单来请求这些声明。  

有关详细信息，请参阅 [Azure AD 中的可选声明](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims)。
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD 支持使用 PKCE 来提高 OAuth 流的安全性

**类型：** 新功能  
**服务类别：** 身份验证（登录）  
**产品功能：** 用户身份验证
 
已更新 Azure AD 文档来指明对 PKCE 的支持。使用 PKCE 可以在执行 OAuth 2.0 授权代码授予流期间提高通信的安全性。  v1.0 和 v2.0 终结点上同时支持 S256 和纯文本 code_challenges。 

有关详细信息，请参阅[请求授权代码](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code)。 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Workday Get_Workers API 中提供预配所有用户属性值的支持

**类型：** 新功能  
**服务类别：** 应用预配  
**产品功能：** 第三方集成
 
从 Workday 到 Active Directory 和 Azure AD 的入站预配公共预览版现在支持提取和预配 Workday Get_Workers API 中可用的所有属性值。 除 Workday 入站预配连接器初始版本随附的属性以外，还支持数百个其他标准和自定义属性。

有关详细信息，请参阅：[自定义 Workday 用户属性列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>将组成员身份从动态更改为静态，或反之

**类型：** 新功能  
**服务类别：** 组管理  
**产品功能：** 协作
 
可更改在组中管理成员身份的方式。 想要在系统中保留相同的组名称和 ID，使针对组的任何现有引用仍然有效时，这很有用；创建新组需要更新这些引用。
我们已更新 Azure AD 管理中心，以支持此功能。 现在，客户可将现有组从动态成员身份转换为分配的成员身份，或反之。 现有的 PowerShell cmdlet 仍可用。

有关详细信息，请参阅[将动态成员身份更改为静态，或反之](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>改进了无缝 SSO 的注销行为

**类型：** 已更改的功能  
**服务类别：** 身份验证（登录）  
**产品功能：** 用户身份验证
 
以前，即使用户显式注销受 Azure AD 保护的应用程序，但如果他们尝试在其企业网络中从已加入域的设备再次访问 Azure AD 应用程序，系统也仍会使用无缝 SSO 自动将其登录。 实施此项更改后，将会支持注销。  这可以让用户选择使用相同或不同的 Azure AD 帐户登录，而不是使用无缝 SSO 自动登录。

有关详细信息，请参阅 [Azure Active Directory 无缝单一登录](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>应用程序代理连接器 1.5.402.0 版已发布

**类型：** 已更改的功能  
**服务类别：** 应用代理  
**产品功能：** 标识安全和保护
 
此连接器版本将在 11 月之前逐步推出。 此新连接器版本包含以下更改：

- 连接器现在会设置域级别而不是子域级别的 Cookie。 这可以确保更顺畅的 SSO 体验，并避免多余的身份验证提示。
- 支持区块编码请求
- 改进了连接器运行状况监视 
- 多项 bug 修复和稳定性改进

有关详细信息，请参阅[了解 Azure AD 应用程序代理连接器](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors)。
 
---

## <a name="february-2018"></a>2018 年 2 月
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>改进了用于管理用户和组的导航界面

**类型：** 更改计划  
**服务类别：** 目录管理  
**产品功能：** 目录

已简化用于管理用户和组的导航体验。 现在，可以从目录概述直接导航到所有用户的列表，并更轻松地访问已删除用户的列表。 还可以从目录概述直接导航到所有组的列表，并更轻松地访问组管理设置。 在目录概述页中，还可以搜索用户、组、企业应用程序或应用注册。 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>世纪互联运营的 Microsoft Azure（Azure 中国区世纪互联）提供登录和审核报告

**类型：** 新功能  
**服务类别：** Azure Stack  
**产品功能：** 监视和报告

世纪互联运营的 Microsoft Azure（Azure 中国区世纪互联）实例现在提供 Azure AD 活动日志报告。 包括以下日志：

- **登录活动日志** - 包括与租户关联的所有登录日志。

- **自助密码审核日志** - 包括所有 SSPR 审核日志。

- **目录管理审核日志** - 包括目录管理（例如用户管理、应用管理等）相关的所有审核日志。

使用这些日志，可以洞察环境的工作情况。 可以将提供的数据用于：

- 确定用户如何使用你的应用和服务。

- 排查妨碍用户完成其工作的问题。

有关如何使用这些报告的详细信息，请参阅 [Azure Active Directory 报告](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)。

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>使用“报告读取者”角色（非管理员角色）查看 Azure AD 活动报告

**类型：** 新功能  
**服务类别：** 报告  
**产品功能：** 监视和报告

由于某些客户反映他们想要启用非管理员角色来访问 Azure AD 活动日志，我们为充当“报告读取者”角色的用户启用了该功能，让他们使用 Azure 门户或图形 API 访问登录和审核活动。 

有关如何使用这些报告的详细信息，请参阅 [Azure Active Directory 报告](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)。 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>以用户属性和用户标识符的形式提供 EmployeeID 声明

**类型：** 新功能  
**服务类别：** 企业应用  
**产品功能：** SSO
 
可以通过企业应用程序 UI，将基于 SAML 登录的应用程序中的成员用户和 B2B 来宾的 **EmployeeID** 配置为用户标识符和用户属性。

有关详细信息，请参阅[在 Azure Active Directory 中为企业应用程序自定义 SAML 令牌中颁发的声明](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)。

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>在 Azure AD 应用程序代理中使用通配符简化了应用程序管理

**类型：** 新功能  
**服务类别：** 应用代理  
**产品功能：** 用户身份验证
 
为了简化应用程序部署并减少管理开销，我们现在支持使用通配符发布应用程序。 若要发布通配符应用程序，可以遵循标准的应用程序发布流，但需要在内部和外部 URL 中使用通配符。

有关详细信息，请参阅 [Azure Active Directory 应用程序代理中的通配符应用程序](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)。

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>开发了新的 cmdlet 用于支持应用程序代理配置

**类型：** 新功能  
**服务类别：** 应用代理  
**产品功能：** 平台

最新版本的 AzureAD PowerShell 预览版模块包含新的 cmdlet，可让客户使用 PowerShell 来配置应用程序代理应用程序。

新 cmdlet 如下： 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>开发了新的 cmdlet 用于支持组配置

**类型：** 新功能  
**服务类别：** 应用代理  
**产品功能：** 平台

最新版本的 AzureAD PowerShell 模块包含用于在 Azure AD 中管理组的 cmdlet。 这些 cmdlet 以前在 AzureADPreview 模块中提供，现已添加到 AzureAD 模块

现已推出正式版的 Goup cmdlet 为： 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>推出了新版 Azure AD Connect

**类型：** 新功能  
**服务类别：** AD Sync  
**产品功能：** 平台
 
Azure AD Connect 是在 Azure AD 与本地数据源（包括 Windows Server Active Directory 和 LDAP）之间同步数据的首选的工具。

>[!Important]
>此版本引入了架构和同步规则更改。 Azure AD Connect 同步服务在升级后将触发完全导入和完全同步步骤。 有关如何更改此行为的信息，请参阅[如何在升级后推迟完全同步](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade)。

此版本提供以下更新和更改：

**已修复的问题**

- 修复了在切换到下一页时，“分区筛选”页的后台任务的计时窗口问题。

- 修复了在 ConfigDB 自定义操作过程中导致访问冲突的 Bug。

- 修复了 Bug，因此可以从 SQL 连接超时恢复。

- 修复了带 SAN 通配符的证书无法通过先决条件检查的 Bug。

- 修复了在 AAD 连接器导出过程中导致 miiserver.exe 崩溃的 Bug。

- 修复了在运行 AAD Connect 向导来更改配置后，可以通过不断地尝试密码登录 DC 的 Bug

**新增功能和改进**
 
- 应用程序遥测 - 管理员可以切换此类数据的开/关设置。

- Azure AD 运行状况数据 - 管理员必须访问运行状况门户才能控制其运行状况设置。 等到服务策略更改以后，代理就会读取并强制实施它。

- 添加了设备写回配置操作以及用于页面初始化的进度栏。

- 改进了 HTML 报表的常规诊断功能以及 ZIP-Text/HTML 报表的完整数据收集功能。

- 提高了自动升级的可靠性并增加了更多的遥测，确保可以确定服务器的运行状况。

- 限制提供给以 AD 连接器帐户为基础的特权帐户的权限。 进行全新安装时，向导会限制特权帐户拥有的针对 MSOL 帐户的权限（前提是 MSOL 帐户已创建）。 这些更改会影响使用 Auto-Create 帐户执行的快速安装和自定义安装。

- 更改了安装程序。在进行 AADConnect 全新安装时，不需要 SA 特权。

- 添加了新的实用工具用于排查特定对象的同步问题。 目前，该实用程序用于检查以下问题：

    - Azure AD 租户中的已同步用户对象和用户帐户之间出现 UserPrincipalName 不匹配的情况。
  
    - 是否已通过域筛选将对象从同步中筛选出来
  
    - 是否已通过组织单位 (OU) 筛选将对象从同步中筛选出来

- 添加了新的实用工具，用于同步当前的密码哈希，该哈希存储在针对特定用户帐户的本地 Active Directory 中。 该实用程序不需要更改密码。 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>添加了支持 Intune 应用保护策略的应用程序以便在基于 Azure AD 应用程序的条件访问中使用

**类型：** 已更改的功能  
**服务类别：** 条件访问  
**产品功能：** 标识安全和保护

我们添加了更多支持基于应用程序的条件访问的应用程序。 现在，可以使用这些批准的客户端应用来访问 Office 365 和其他已连接到 Azure AD 的云应用。

在 2 月底之前将添加以下应用程序：

- Microsoft Power BI

- Microsoft Launcher

- Microsoft Invoicing

有关详细信息，请参阅：

- [批准的客户端应用要求](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD 基于应用的条件访问](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>移动体验的使用条款更新 

**类型：** 已更改的功能  
**服务类别：** 使用条款  
**产品功能：** 符合性

显示使用条款时，现在可以单击“浏览时遇到问题？请单击此处”。 单击此链接会在设备本地打开使用条款。 不管文档中的字体大小或设备屏幕大小如何，都可以根据需要进行缩放，以方便阅读文档。 

---
 
## <a name="january-2018"></a>2018 年 1 月
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD 应用库中提供了新的联合应用 

**类型：** 新功能  
**服务类别：** 企业应用  
**产品功能：** 第三方集成

2018 年 1 月，应用库中添加了支持联合的以下新应用：

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698)、[OneTrust 隐私管理软件](https://go.microsoft.com/fwlink/?linkid=861660)、[Dealpath](https://go.microsoft.com/fwlink/?linkid=863526)IriusRisk Federated Directory 和 [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701)。

有关这些应用的详细信息，请参阅 [SaaS 应用程序与 Azure Active Directory 集成](https://aka.ms/appstutorial)。

有关在 Azure AD 应用库中列出应用程序的详细信息，请参阅[在 Azure Active Directory 应用程序库中列出应用程序](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>登录时检测到其他风险

**类型：** 新功能  
**服务类别：** 标识保护  
**产品功能：** 标识安全和保护

从检测到的风险事件获得的见解会绑定到 Azure AD 订阅。 使用 Azure AD Premium P2 版本时，可以获取有关所有基础检测的最详细的信息。

使用 Azure AD Premium P1 版本时，许可证未涵盖的检测项会显示为风险事件“登录时检测到其他风险”。

有关详细信息，请参阅 [Azure Active Directory 风险事件](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events)。
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>在最终用户的访问面板中隐藏 Office 365 应用程序

**类型：** 新功能  
**服务类别：** 我的应用  
**产品功能：** SSO

现在，可以通过新的用户设置来更好地管理 Office 365 应用程序显示用户访问面板的方式。 如果只想在 Office 门户中显示 Office 应用，可以借助此选项来减少用户访问面板中的应用数量。 该设置位于“用户设置”中，带有“用户只能在 Office 365 门户中查看 Office 365 应用”标签。

有关详细信息，请参阅[在 Azure Active Directory 的用户体验中隐藏应用程序](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)。

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>直接从应用的 URL 无缝登录到启用了密码 SSO 的应用 

**类型：** 新功能  
**服务类别：** 我的应用  
**产品功能：** SSO

现已通过一个便捷工具提供“我的应用”浏览器扩展。该工具以快捷方式的形式显示在浏览器中，可实现“我的应用”单一登录功能。 安装后，用户将在浏览器中看到一个堆积圆点图标，单击该图标可快速访问应用。 用户现在可以：

- 从应用登录页直接登录到基于密码 SSO 的应用
- 使用快速搜索功能启动任何应用
- 在扩展中使用快捷方式访问最近使用的应用
- 该扩展适用于 Microsoft Edge、Chrome 和 Firefox。
 
有关详细信息，请参阅[我的应用安全登录扩展](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension)。

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure 经典门户中的 Azure AD 管理体验已停用

**类型：** 已弃用   
**服务类别：** Azure AD  
**产品功能：** 目录

从 2018 年 1 月 8 日起，Azure 经典门户中的 Azure AD 管理体验已停用。 Azure 经典门户本身在同一时间也已停用。 今后，应使用 [Azure AD 管理员中心](https://aad.portal.azure.com)来执行所有基于门户的 Azure AD 管理。
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor Web 门户已停用

**类型：** 已弃用  
**服务类别：** Azure AD  
**产品功能：** 目录
 
从 2018 年 1 月 8 日起，PhoneFactor Web 门户已停用。 此门户用于管理 MFA 服务器，但这些功能已移至 Azure 门户 (portal.azure.com)。 

MFA 配置位于：“Azure Active Directory”\>“MFA 服务器”
 
---
 
### <a name="deprecate-azure-ad-reports"></a>弃用 Azure AD 报告

**类型：** 已弃用  
**服务类别：** 报告  
**产品功能：** 标识生命周期管理  


随着新 Azure Active Directory 管理控制台的正式发布以及用于活动和安全报告的新 API 的推出，“/reports”终结点下面的报告 API 从 2017 年 12 月 31 日开始已停用。

**可用功能**

在过渡到新管理控制台的过程中，我们开发了 2 个新 API 用于检索 Azure AD 活动日志。 新的 API 集提供更丰富的筛选和排序功能，此外还提供更丰富的审核和登录活动。 现在可以通过 Microsoft Graph 中的 Identity Protection 风险事件 API 来访问以前通过安全报告提供的数据。

有关详细信息，请参阅：

- [Azure Active Directory 报告 API 入门](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Azure Active Directory Identity Protection 和 Microsoft Graph 入门](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>2017 年 12 月

### <a name="terms-of-use-in-the-access-panel"></a>访问面板中的使用条款

**类型：** 新功能  
**服务类别：** 使用条款  
**产品功能：** 符合性
 
现在，可以转到访问面板并查看以前接受的使用条款。

执行以下步骤:

1. 转到 [MyApps 门户](https://myapps.microsoft.com)并登录。

2. 在右上角选择自己的姓名，然后从列表中选择“个人资料”。 

3. 在“个人资料”中选择“查看使用条款”。 

4. 现在，可以查看已接受的使用条款。 

有关详细信息，请参阅 [Azure AD 使用条款功能（预览版）](https://docs.microsoft.com/azure/active-directory/active-directory-tou)。
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>新 Azure AD 登录体验

**类型：** 新功能  
**服务类别：** Azure AD  
**产品功能：** 用户身份验证
 
Azure AD 和 Microsoft 帐户标识系统 UI 经过重新设计，现在拥有一致的外观。 此外，Azure AD 登录页会先收集用户名，然后在第二个屏幕上收集凭据。

有关详细信息，请参阅[现已推出新 Azure AD 登录体验的公共预览版](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/)。
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>更少的登录提示：Azure AD 登录的新“使我保持登录状态”体验

**类型：** 新功能  
**服务类别：** Azure AD  
**产品功能：** 用户身份验证
 
Azure AD 登录页上的“使我保持登录状态”复选框已被替换为新提示，成功通过身份验证后会显示该提示。 

如果对此提示回答“是”，则服务会提供永久刷新令牌。 此行为与在旧体验中选中“使我保持登录状态”复选框时相同。 对于联合租户，此提示在使用联合服务成功完成身份验证后显示。

有关详细信息，请参阅[更少的登录提示：现已推出 Azure AD 新的“使我保持登录状态”体验的预览版](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/)。 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>添加配置以要求在接受使用条款之前先将其展开。

**类型：** 新功能  
**服务类别：** 使用条款  
**产品功能：** 符合性
 
为管理员添加了一个选项，要求其用户在接受使用条款之前先将其展开。

对于要求用户展开使用条款这一选项，可选择“打开”或“关闭”。 “打开”设置要求用户在接受使用条款之前先查看条款。

有关详细信息，请参阅 [Azure AD 使用条款功能（预览版）](https://docs.microsoft.com/azure/active-directory/active-directory-tou)。
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>符合条件的角色分配的作用域激活

**类型：** 新功能  
**服务类别：** Privileged Identity Management  
**产品功能：** Privileged Identity Management
 
可以使用范围激活功能来激活符合条件的 Azure 资源角色分配，其自治性比原始分配的默认值小。 例如，你被分配为租户中某个订阅的所有者。 使用范围激活可以激活订阅中包含的最多五个资源（例如资源组和虚拟机）的“所有者”角色。 划分激活范围可能会降低对关键 Azure 资源执行不必要更改的可能性。

有关详细信息，请参阅[什么是 Azure AD Privileged Identity Management？](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)。
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD 应用库中提供了新的联合应用

**类型：** 新功能  
**服务类别：** 企业应用  
**产品功能：** 第三方集成

我们已于 2017 年 12 月将这些支持联合的新应用添加到了我们的应用库中：

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523)、Adobe Experience Manager、[EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685)、[Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe、[FactSet](https://go.microsoft.com/fwlink/?linkid=863525)、[IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517)、[MOBI](https://go.microsoft.com/fwlink/?linkid=863521)、[MobileIron Azure AD 集成](https://go.microsoft.com/fwlink/?linkid=858027)、[Reflektive](https://go.microsoft.com/fwlink/?linkid=863518)、[SAML SSO for Bamboo by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863520)、[SAML SSO for Bitbucket by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863519)、[Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522)、WebHR、Zenegy Azure AD 集成。

有关这些应用的详细信息，请参阅 [SaaS 应用程序与 Azure Active Directory 集成](https://aka.ms/appstutorial)。

要详细了解如何在 Azure AD 应用库中列出应用程序，请参阅[在 Azure Active Directory 应用程序库中列出应用程序](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD 目录角色的审批工作流

**类型：** 已更改的功能  
**服务类别：** Privileged Identity Management  
**产品功能：** Privileged Identity Management
 
Azure AD 目录角色的审批工作流程已正式发布。

通过审批工作流程，特权角色管理员可要求符合条件的角色成员先请求角色激活才能使用特权角色。 可向多个用户和组委派审批职责。 符合条件的角色成员会在审批完成且角色激活时收到通知。

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>传递身份验证 - Skype for Business 支持

**类型：** 已更改的功能  
**服务类别：** 身份验证（登录）  
**产品功能：** 用户身份验证

传递身份验证现在支持用户登录到支持新式身份验证的 Skype for Business 客户端应用程序，包括联机和混合拓扑。 

有关详细信息，请参阅[支持新式身份验证的 Skype for Business 拓扑](https://technet.microsoft.com/library/mt803262.aspx)。
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>更新到 Azure RBAC 的 Azure AD Privileged Identity Management（预览版）

**类型：** 已更改的功能  
**服务类别：** Privileged Identity Management  
**产品功能：** Privileged Identity Management
 
通过 Azure 基于角色的访问控制 (RBAC) 的 Azure AD Privileged Identity Management (PIM) 公共预览版刷新，现在可以：

* 使用 Just Enough Administration。
* 需要批准才能激活资源角色。
* 安排将来激活需要批准 Azure AD 和 Azure RBAC 角色的角色。
 
有关详细信息，请参阅 [Azure 资源的 Privileged Identity Management（预览版）](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)。

---
 
## <a name="november-2017"></a>2017 年 11 月
 
### <a name="access-control-service-retirement"></a>访问控制服务停用

**类型：** 更改计划  
**服务类别：** 访问控制服务  
**产品功能：** 访问控制服务 

Azure Active Directory 访问控制（也称作访问控制服务）将在 2018 年底停用。 接下来的几周内会提供更多信息，包括详细的计划和高级迁移指南。 有关访问控制服务的任何问题，请在本页面留言，团队成员将予以解答。

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>限制对 Intune 托管浏览器的浏览器访问 

**类型：** 更改计划  
**服务类别：** 条件访问  
**产品功能：** 标识安全和保护

可以使用 Intune 托管浏览器作为批准的应用，限制对 Office 365 以及其他已连接 Azure AD 的云应用的浏览器访问。 

现在，可为基于应用程序的条件访问配置以下条件：

**客户端应用：** 浏览器

**此更改会产生什么影响？**

目前，使用此条件时会阻止访问。 推出预览版后，所有访问都需要使用托管的浏览器应用程序。 

可在即将发布的博客和发行说明中了解此功能和详细信息。 

有关详细信息，请参阅 [Azure AD 中的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>新批准的客户端应用，适用于基于 Azure AD 应用的条件访问

**类型：** 更改计划  
**服务类别：** 条件访问  
**产品功能：** 标识安全和保护

以下应用包含在[批准的客户端应用](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)列表中：

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)

有关详细信息，请参阅：

- [批准的客户端应用要求](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD 基于应用的条件访问](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>使用条款支持多种语言

**类型：** 新功能    
**服务类别：** 使用条款  
**产品功能：** 符合性

管理员现在可以创建包含多个 PDF 文档的新使用条款。 可以使用相应语言来标记这些 PDF 文档。 将会根据用户的偏好，以匹配的语言显示 PDF。 如果没有匹配的语言，则以默认语言显示。

---
 
### <a name="real-time-password-writeback-client-status"></a>实时密码写回客户端状态

**类型：** 新功能  
**服务类别：** 自助密码重置  
**产品功能：** 用户身份验证

现在可以查看本地密码写回客户端的状态。 此选项位于[密码重置](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset)页的“本地集成”部分。 

如果与本地写回客户端的连接出现问题，将会显示一条错误消息，其中包含：

- 有关无法连接到本地写回客户端的原因的信息。
- 可帮助解决此问题的文档链接。 

有关详细信息，请参阅[本地集成](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration)。

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD 基于应用的条件访问 
 
**类型：** 新功能  
**服务类别：** Azure AD  
**产品功能：** 标识安全和保护

现在可将对 Office 365 和其他已连接 Azure AD 的云应用的访问权限限于[批准的客户端应用](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)，这些应用支持使用[基于 Azure AD 的应用的条件访问](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)的 Intune 应用保护策略。 Intune 应用保护策略用于配置和保护这些客户端应用程序中的公司数据。

通过组合[基于应用](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)的条件访问策略和[基于设备](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications)的条件访问策略，可以灵活地保护个人和公司设备的数据。

以下条件和控制现可用于基于应用的条件访问：

**支持的平台条件**

- iOS
- Android

**客户端应用条件**

- 移动应用和桌面客户端

**访问控制**

- 需要批准的客户端应用

有关详细信息，请参阅[基于 Azure AD 应用的条件访问](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)。
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>在 Azure 门户中管理 Azure AD 设备

**类型：** 新功能  
**服务类别：** 设备注册和管理  
**产品功能：** 标识安全和保护

现在可在一个位置找到连接到 Azure AD 的所有设备以及与设备相关的活动。 在 Azure 门户中管理所有设备标识和设置有新的管理体验。 在此版本中，可以：

- 查看可用于 Azure AD 中的条件访问的所有设备。
- 查看属性，包括已加入混合 Azure AD 的设备。
- 查找已加入 Azure AD 的设备的 BitLocker 密钥、使用 Intune 管理设备，等等。
- 管理与 Azure AD 设备相关的设置。

有关详细信息，请参阅[使用 Azure 门户管理设备](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal)。

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>支持将 macOS 作为 Azure AD 条件访问的设备平台 

**类型：** 新功能    
**服务类别：** 条件访问  
**产品功能：** 标识安全和保护 

现在可以包含（或排除）macOS 作为 Azure AD 条件访问策略中的设备平台条件。 通过将 macOS 添加到支持的设备平台，可以：

- **使用 Intune 登记和管理 macOS 设备。** 类似于 iOS 和 Android 等其他平台，macOS 可通过公司门户应用程序执行统一注册。 可以使用适用于 macOS 的新公司门户应用，通过 Intune 登记设备并将其注册到 Azure AD。
- **确保 macOS 设备符合组织在 Intune 中定义的符合性策略。** 可以在 Azure 门户上的 Intune 中设置 macOS 设备的符合性策略。 
- **只允许符合条件的 macOS 设备访问 Azure AD 中的应用程序。** 条件访问策略创作将 macOS 用作单独的设备平台选项。 现在，可以针对 Azure 中设置的目标应用程序创作特定于 macOS 的条件访问策略。

有关详细信息，请参阅：

- [使用 Intune 创建适用于 macOS 设备的设备符合性策略](https://aka.ms/macoscompliancepolicy)
- [Azure AD 中的条件访问](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>适用于 Azure 多重身份验证的网络策略服务器扩展 

**类型：** 新功能    
**服务类别：** 多重身份验证  
**产品功能：** 用户身份验证

适用于 Azure 多重身份验证的网络策略服务器扩展使用现有的服务器在身份验证基础结构中添加基于云的多重身份验证功能。 使用网络策略服务器扩展，可将电话呼叫、短信或电话应用验证添加到现有的身份验证流。 无需安装、配置和维护新服务器。 

此扩展是为想要保护虚拟专用网络连接，但不部署 Azure 多重身份验证服务器的组织创建的。 网络策略服务器扩展充当 RADIUS 与基于云的 Azure 多重身份验证之间的适配器，以为联合用户或已同步用户提供身份验证的第二个因素。

有关详细信息，请参阅[将现有网络策略服务器基础结构与 Azure 多重身份验证集成](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension)。
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>还原或永久删除已删除的用户

**类型：** 新功能    
**服务类别：** 用户管理  
**产品功能：** 目录 

在 Azure AD 管理中心，可以：

- 还原已删除的用户。 
- 永久删除用户。

**试试看：**

1. 在 Azure AD 管理中心，选择“管理”部分中的“[所有用户](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All)”。 

2. 从“显示”列表中，选择“最近删除的用户”。 

3. 选择一个或多个最近删除的用户，然后将其还原或永久删除。
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>新批准的客户端应用，适用于基于 Azure AD 应用的条件访问
 
**类型：** 已更改的功能  
**服务类别：** 条件访问  
**产品功能：** 标识安全和保护

以下应用已添加到[批准的客户端应用](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)列表：

- Microsoft Planner
- Azure 信息保护 

有关详细信息，请参阅：

- [批准的客户端应用要求](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD 基于应用的条件访问](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>在条件访问策略中的控制条件之间使用“OR” 

**类型：** 已更改的功能    
**服务类别：** 条件访问  
**产品功能：** 标识安全和保护
 
现在，可对条件访问控制使用“OR”（需要一个选定控制条件）。 可以使用此功能创建在访问控制条件之间包含“OR”的策略。 例如，可以使用此功能创建一个策略，要求用户使用多重身份验证登录，或要求用户在符合条件的设备上操作。

有关详细信息，请参阅 [Azure AD 条件访问中的控制](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)。
 
---

### <a name="aggregation-of-real-time-risk-events"></a>实时风险事件的聚合

**类型：** 已更改的功能    
**服务类别：** 标识保护  
**产品功能：** 标识安全和保护

在 Azure AD Identity Protection 中，某一天源自同一 IP 地址的所有实时风险事件现按照每种风险事件类型聚合在一起。 此更改限制了显示的风险事件数量，但不会对用户安全性造成任何影响。

每当用户登录时，基础实时检测都会运行。 如果针对多重身份验证或阻止访问设置了登录风险安全策略，则每次存在风险的登录期间都会触发该设置。
 
---
 
## <a name="october-2017"></a>2017 年 10 月

### <a name="deprecate-azure-ad-reports"></a>弃用 Azure AD 报告

**类型：** 更改计划  
**服务类别：** 报告  
**产品功能：** 标识生命周期管理  

Azure 门户提供：

- 新的 Azure AD 管理控制台。
- 用于活动和安全报告的新 API。
 
由于推出了这些新功能，/reports 终结点下的报告 API 将在 2017 年 12 月 10 日停用。 

---

### <a name="automatic-sign-in-field-detection"></a>自动登录字段检测

**类型：** 已修复   
**服务类别：** 我的应用  
**产品功能：** 单一登录  

对于显示 HTML 用户名和密码字段的应用程序，Azure AD 支持自动登录字段检测。 [如何自动捕获应用程序的登录字段](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application)中介绍了这些步骤。 在 [Azure 门户](https://aad.portal.azure.com)中的“企业应用程序”页面上添加一个“非库”应用程序，即可找到此功能。 此外，可在此新应用程序中将“单一登录”模式配置为“基于密码的单一登录”，输入 Web URL，然后保存页面。
 
由于某个服务问题，此功能曾经暂时禁用过。 该问题现已得到解决，自动登录字段检测功能再次可用。

---

### <a name="new-multi-factor-authentication-features"></a>新的多重身份验证功能

**类型：** 新功能  
**服务类别：** 多重身份验证  
**产品功能：** 标识安全和保护  

多重身份验证 (MFA) 是保护组织不可或缺的组成部分。 为使凭证的适应能力更强，体验更顺畅，添加了以下功能： 

- 将多重身份验证质询结果直接集成到 Azure AD 登录报告，包括以编程方式访问 MFA 结果。
- 将 MFA 配置更深入地集成到 Azure 门户中的 Azure AD 配置体验。

在此公共预览版中，MFA 管理和报告集成到核心 Azure AD 配置体验中。 现在，可以在 Azure AD 体验中管理 MFA 管理门户功能。

有关详细信息，请参阅 [Azure 门户中的 MFA 报告参考](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa)。 

---

### <a name="terms-of-use"></a>使用条款

**类型：** 新功能  
**服务类别：** 使用条款  
**产品功能：** 符合性  

可以使用 Azure AD 的使用条款功能向用户显示法律要求或符合性要求的相关免责声明。

可在以下情况下使用 Azure AD 使用条款：

- 针对组织中所有用户的常规使用条款
- 基于用户属性（例如，动态组中的医生与护士或国内员工和国际员工）的具体使用条款
- 有关访问业务影响性较高的应用（例如 Salesforce）的具体使用条款

有关详细信息，请参阅 [Azure AD 使用条款](https://docs.microsoft.com/azure/active-directory/active-directory-tou)。

---

### <a name="enhancements-to-privileged-identity-management"></a>Privileged Identity Management 的增强

**类型：** 新功能  
**服务类别：** Privileged Identity Management  
**产品功能：** Privileged Identity Management  

使用 Azure AD Privileged Identity Management，可以管理、控制和监视对组织中 Azure 资源（预览版）的访问：

- 订阅
- 资源组
- 虚拟机 

Azure 门户中使用 Azure RBAC 功能的所有资源都可以利用 Azure AD Privileged Identity Management 提供的所有安全和生命周期管理功能。

有关详细信息，请参阅 [Azure 资源的 Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)。

---

### <a name="access-reviews"></a>访问评审

**类型：** 新功能  
**服务类别：** 访问评审  
**产品功能：** 符合性  

组织可以使用访问评审（预览版）有效管理组成员身份以及对企业应用程序的访问权限： 

- 可以使用访问评审鉴定来宾用户对应用程序的访问权限及其组成员身份。 评审者可以根据访问评审提供的见解，有效决定是否允许来宾继续访问。
- 可以使用访问评审再次验证员工对应用程序和组成员身份的访问权限。

可以收集与组织相关的程序中的访问评审控件，跟踪符合性或风险敏感应用程序的审阅。

有关详细信息，请参阅 [Azure AD 访问评审](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview)。

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>在“我的应用”和 Office 365 应用启动器中隐藏第三方应用程序

**类型：** 新功能  
**服务类别：** 我的应用  
**产品功能：** 单一登录  

现在，可以通过“隐藏应用”属性更好地管理用户门户中显示的应用。 如果为后端服务显示的应用磁贴或重复的磁贴导致用户的应用启动器变得混杂，隐藏应用可帮助解决问题。 切换开关位于第三方应用的“属性”部分中，带有“对用户可见?”标签 还可以通过 PowerShell 以编程方式隐藏应用。 

有关详细信息，请参阅[在 Azure AD 的用户体验中隐藏第三方应用程序](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)。 


**可用功能**

 在过渡到新管理控制台的过程中，开发了两个新 API 用于检索可用的 Azure AD 活动日志。 新的 API 集提供更丰富的筛选和排序功能，此外还提供更丰富的审核和登录活动。 现在可以通过 Microsoft Graph 中的 Identity Protection 风险事件 API 来访问以前通过安全报告提供的数据。


## <a name="september-2017"></a>2017 年 9 月

### <a name="hotfix-for-identity-manager"></a>适用于 Identity Manager 的修补程序

**类型：** 已更改的功能  
**服务类别：** Identity Manager  
**产品功能：** 标识生命周期管理  

现已推出 Identity Manager 2016 Service Pack 1 截止 2017 年 9 月 25 日的修补程序汇总包（内部版本 4.4.1642.0）。 此汇总包：

- 解决了一些问题并添加了改进项。
- 是一项累积更新，可取代 Identity Manager 2016 内部版本 4.4.1459.0 及其之前的所有 Identity Manager 2016 Service Pack 1 更新。 
- 要求安装 Identity Manager 2016 内部版本 4.4.1302.0。 

有关详细信息，请参阅[为 Identity Manager 2016 Service Pack 1 推出了修补程序汇总包（内部版本 4.4.1642.0）](https://support.microsoft.com/help/4021562)。 

---