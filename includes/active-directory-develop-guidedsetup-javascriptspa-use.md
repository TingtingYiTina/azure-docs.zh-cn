---
title: include 文件
description: include 文件
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 496a24f79cee1323077c357789628123d082ceac
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "51716134"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>使用 Microsoft 身份验证库 (MSAL) 登录用户

1. 将以下代码添加到 `index.html` 文件的 `<script></script>` 标记中：

```javascript
//Pass null for default authority (https://login.microsoftonline.com/common)
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, null, acquireTokenRedirectCallBack,
    {storeAuthStateInCookie: true, cacheLocation: "localStorage"});

function signIn() {
    myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
        //Login Success
        showWelcomeMessage();
        acquireTokenPopupAndCallMSGraph();
    }, function (error) {
        console.log(error);
    });
}

function acquireTokenPopupAndCallMSGraph() {
    //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
    myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
        callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
    }, function (error) {
        console.log(error);
        // Call acquireTokenPopup (popup window) in case of acquireTokenSilent failure due to consent or interaction required ONLY
        if (error.indexOf("consent_required") !== -1 || error.indexOf("interaction_required") !== -1 || error.indexOf("login_required") !== -1) {
            myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
                callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
            }, function (error) {
                console.log(error);
            });
        }
    });
}

function graphAPICallback(data) {
    //Display user data on DOM
    var divWelcome = document.getElementById('WelcomeMessage');
    divWelcome.innerHTML += " to Microsoft Graph API!!";
    document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
}

function showWelcomeMessage() {
    var divWelcome = document.getElementById('WelcomeMessage');
    divWelcome.innerHTML += 'Welcome ' + myMSALObj.getUser().name;
    var loginbutton = document.getElementById('SignIn');
    loginbutton.innerHTML = 'Sign Out';
    loginbutton.setAttribute('onclick', 'signOut();');
}

// This function can be removed if you do not need to support IE
function acquireTokenRedirectAndCallMSGraph() {
    //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
    myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
      callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
    }, function (error) {
        console.log(error);
        //Call acquireTokenRedirect in case of acquireToken Failure
        if (error.indexOf("consent_required") !== -1 || error.indexOf("interaction_required") !== -1 || error.indexOf("login_required") !== -1) {
            myMSALObj.acquireTokenRedirect(applicationConfig.graphScopes);
        }
    });
}

function acquireTokenRedirectCallBack(errorDesc, token, error, tokenType)
{
 if(tokenType === "access_token")
 {
     callMSGraph(applicationConfig.graphEndpoint, token, graphAPICallback);
 } else {
     console.log("token type is:"+tokenType);
 }
}


// Browser check variables
var ua = window.navigator.userAgent;
var msie = ua.indexOf('MSIE ');
var msie11 = ua.indexOf('Trident/');
var msedge = ua.indexOf('Edge/');
var isIE = msie > 0 || msie11 > 0;
var isEdge = msedge > 0;

//If you support IE, our recommendation is that you sign-in using Redirect APIs
//If you as a developer are testing using Edge InPrivate mode, please add "isEdge" to the if check
if (!isIE) {
    if (myMSALObj.getUser()) {// avoid duplicate code execution on page load in case of iframe and popup window.
        showWelcomeMessage();
        acquireTokenPopupAndCallMSGraph();
    }
}
else {
    document.getElementById("SignIn").onclick = function () {
        myMSALObj.loginRedirect(applicationConfig.graphScopes);
    };
    if (myMSALObj.getUser() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
        showWelcomeMessage();
        acquireTokenRedirectAndCallMSGraph();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>更多信息

用户首次单击“登录”按钮后，`signIn` 方法将调用 `loginPopup` 以登录用户。 此方法导致使用 *Microsoft Azure Active Directory v2.0 终结点*打开一个弹出窗口，以提示用户输入凭据并验证该凭据。 登录成功后，用户被重定向回原始的 index.html 页面，并接收到一个由 `msal.js` 处理的令牌，令牌中包含的信息被缓存。 该令牌称为 ID令牌，并包含有关用户的基本信息，如用户显示名。 如果计划将此令牌提供的数据用于任何目的，则需确保此令牌已由后端服务器验证，以保证将令牌颁发给应用程序的有效用户。

本指南生成的 SPA 调用 `acquireTokenSilent` 和/或 `acquireTokenPopup` 来获取用于查询 Microsoft Graph API 以获取用户配置文件信息的访问令牌。 如果需要验证 ID 令牌的示例，请查看 GitHub 中的[此](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 示例")示例应用程序 - 该示例使用 ASP.NET Web API 进行令牌验证。

#### <a name="getting-a-user-token-interactively"></a>以交互方式获取用户令牌

初次登录后，你不希望在用户每次需要请求令牌访问资源时都要求其重新认证，因此大部分时间都应使用 acquireTokenSilent 来获取令牌。 但有些情况下，需要强制用户与 Azure Active Directory v2.0 终结点交互，一些示例包括：

- 由于密码已过期，用户可能需要重新输入凭据
- 应用程序正在请求访问用户需要同意的资源
- 需要双重身份验证

调用 acquireTokenPopup(scope) 导致显示一个弹出窗口（或调用 acquireTokenRedirect(scope) 导致将用户重定向到 Azure Active Directory v2.0 终结点），在这种情况下，用户需要通过确认其凭据、同意所需资源或完成双因素身份验证来与之交互。

#### <a name="getting-a-user-token-silently"></a>以静默方式获取用户令牌

` acquireTokenSilent` 方法处理令牌获取和续订，无需进行任何用户交互。 首次执行 `loginPopup`（或 `loginRedirect`）后，通常使用 `acquireTokenSilent` 方法获取用于访问受保护资源的令牌，以便进行后续调用，因为调用请求或续订令牌都以静默方式进行。
在某些情况下，`acquireTokenSilent` 可能会失败，例如用户的密码已过期。 应用程序可以通过两种方式处理此异常：

1. 立即调用 `acquireTokenPopup`，随后出现用户登录提示。 此模式通常用于联机应用程序，此时应用程序中没有可供用户使用的未经身份验证的内容。 本指导式设置生成的示例使用此模式。

2. 应用程序还可以直观地提示用户以交互方式登录，用户可以选择在合适的时间登录，或者应用程序可以稍后重试 `acquireTokenSilent`。 如果用户可以在不中断应用程序的情况下（例如，应用程序中有可用的未经身份验证的内容）使用应用程序的其他功能，则通常会使用此方法。 在这种情况下，用户可以决定何时登录并访问受保护的资源，或何时刷新过期信息。

> [!NOTE]
> 由于存在与 Internet Explorer 浏览器处理弹出窗口有关的[已知问题](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser)，当使用的浏览器是 Internet Explorer 时，上面的代码使用 `loginRedirect` 和 `acquireTokenRedirect` 方法。
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>使用刚获得的令牌调用 Microsoft Graph API

将以下代码添加到 `index.html` 文件的 `<script></script>` 标记中：

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>对受保护 API 进行 REST 调用的详细信息

在本指南创建的示例应用程序中，使用 `callMSGraph()` 方法对需要令牌的受保护资源发出 HTTP `GET` 请求，然后将内容返回给调用方。 此方法可在 HTTP 授权标头中添加获取的令牌。 本指南创建的示例应用程序中的资源是 Microsoft Graph API me 终结点，它显示用户个人资料信息。

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>添加方法以注销用户

将以下代码添加到 `index.html` 文件的 `<script></script>` 标记中：

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```
