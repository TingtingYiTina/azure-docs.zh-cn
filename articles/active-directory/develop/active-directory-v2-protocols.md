---
title: 了解 Azure AD v2.0 支持的授权协议 | Microsoft Docs
description: 有关 Azure AD v2.0 终结点支持的协议的指南。
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 57a3d5fc50c2278b34fddbfba61b12b0d81a33ed
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52424521"
---
# <a name="v20-protocols---oauth-20--openid-connect"></a>v2.0 协议 - OAuth 2.0 和 OpenID Connect

v2.0 终结点可以使用 Azure AD，通过行业标准协议（OpenID Connect 与 OAuth 2.0）提供标识即服务。 尽管此服务与标准兼容，但这些协议的两个实现之间仍然存在微妙的差异。 如果选择通过直接发送和处理 HTTP 请求，或使用第三方开放源代码库来编写代码，而不是使用我们的其中一个[开放源代码库](reference-v2-libraries.md)，则可以参考此处提供的有用信息。

> [!NOTE]
> v2.0 终结点并不支持所有 Azure Active Directory 方案和功能。 若要确定是否应使用 v2.0 终结点，请阅读 [v2.0 限制](active-directory-v2-limitations.md)。

## <a name="the-basics"></a>基础知识

几乎在所有的 OAuth 和 OpenID Connect 流中，都有四个参与交换的对象：

![OAuth 2.0 角色](../../media/active-directory-v2-flows/protocols_roles.png)

* **授权服务器**是 v2.0 终结点。 它负责确保用户的标识、授予和吊销对资源的访问权限，以及颁发令牌。 它也称为标识提供者：安全处理与用户信息、用户访问权，以及流中合作对象彼此间信任关系有关的任何项目。
* **资源所有者**通常是最终用户。 它是拥有数据的一方，并且有权允许第三方访问该数据或资源。
* **OAuth 客户端**是应用，按照其应用程序 ID 进行标识。它通常是与最终用户交互的对象，并向授权服务器请求令牌。 客户端必须获得资源所有者授权才能访问资源。
* **资源服务器**是资源或数据所在的位置。 它信任授权服务器安全验证和授权 OAuth 客户端，并使用 Bearer access_token 来确保可以授予对资源的访问权限。

## <a name="app-registration"></a>应用注册
所有使用 v2.0 终结点的应用都必须先在 [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) 上注册，才能使用 OAuth 或 OpenID Connect 进行交互。 应用注册进程会收集一些值并将其分配到应用：

* 用于唯一标识应用的 **应用程序 ID**
* 用于将响应定向回应用的**重定向 URI** 或**包标识符**
* 其他一些特定于方案的值。

请了解如何 [注册应用](quickstart-v2-register-an-app.md)获取详细信息。

## <a name="endpoints"></a>终结点

注册后，应用将通过向 v2.0 终结点发送请求来与 Azure AD 通信：

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

其中 `{tenant}` 可以接受以下四个不同值之一：

| 值 | Description |
| --- | --- |
| `common` |允许用户使用个人的 Microsoft 帐户和工作/学校帐户从 Azure Active Directory 登录应用程序。 |
| `organizations` |仅允许用户使用工作/学校帐户从 Azure Active Directory 登录应用程序。 |
| `consumers` |仅允许用户使用个人的 Microsoft 帐户 (MSA) 登录应用程序。 |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` 或 `contoso.onmicrosoft.com` |仅允许用户使用工作/学校帐户从特定的 Azure Active Directory 租户登录应用程序。 可以使用 Azure AD 租户的友好域名或租户的 GUID 标识符。 |

有关如何与这些终结点交互的详细信息，请选择以下特定的应用类型。

## <a name="tokens"></a>令牌

OAuth 2.0 和 OpenID Connect 的 v2.0 实现广泛使用了持有者令牌，包括表示为 JWT 的持有者令牌。 持有者令牌是一种轻型安全令牌，它授予对受保护资源的“持有者”访问权限。 从这个意义上来说，“持有者”是可以提供令牌的任何一方。 虽然某一方必须首先通过 Azure AD 的身份验证才能收到持有者令牌，但如果不采取必要的步骤在传输过程和存储中对令牌进行保护，令牌可能会被意外的某一方拦截并使用。 虽然某些安全令牌具有内置机制来防止未经授权方使用它们，但是持有者令牌没有这一机制，因此必须在安全的通道（例如传输层安全 (HTTPS)）中进行传输。 如果持有者令牌以明文传输，则恶意方可以利用中间人攻击来获得令牌并使用它来对受保护资源进行未经授权的访问。 当存储或缓存持有者令牌供以后使用时，也应遵循同样的安全原则。 请始终确保应用以安全的方式传输和存储持有者令牌。 有关持有者令牌的更多安全注意事项，请参阅 [RFC 6750 第 5 部分](https://tools.ietf.org/html/rfc6750)。

有关 v2.0 终结点中使用的不同类型令牌的更多详细信息，请参阅 [v2.0 终结点令牌参考](v2-id-and-access-tokens.md)。

## <a name="protocols"></a>协议

如果已准备好查看部分示例请求，请从下列教程之一开始。 每个教程对应于特定的身份验证方案。 如果在确定适当的流时需要帮助，请查看[可使用 v2.0 构建的应用类型](v2-app-types.md)。

* [使用 OAuth 2.0 构建移动和本机应用程序](v2-oauth2-auth-code-flow.md)
* [使用 OpenID Connect 构建 Web 应用](v2-protocols-oidc.md)
* [使用 OAuth 2.0 隐式流构建单页应用](v2-oauth2-implicit-grant-flow.md)
* [使用 OAuth 2.0 客户端凭据流构建守护程序或服务器端进程](v2-oauth2-client-creds-grant-flow.md)
* [使用 OAuth 2.0 代理流在 Web API 中获取令牌](v2-oauth2-on-behalf-of-flow.md)
