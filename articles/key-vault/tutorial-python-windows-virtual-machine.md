---
title: 教程 - 如何将 Azure Key Vault 与通过 Python 编写的 Azure Windows 虚拟机配合使用 | Microsoft Docs
description: 教程：将 ASP.NET Core 应用程序配置为从 Key Vault 读取机密
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 26b5b16e3eb016edbe53c3526e51c3aa44f307b5
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52583578"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-windows-virtual-machine-in-python"></a>教程：如何将 Azure Key Vault 与通过 Python 编写的 Azure Windows 虚拟机配合使用

Azure Key Vault 用于保护机密，例如访问应用程序、服务和 IT 资源所需的 API 密钥、数据库连接字符串。

本教程演练将 Azure Web 应用程序配置为使用 Azure 资源的托管标识，以从 Azure Key Vault 读取信息所要执行的步骤。 本教程基于 [Azure Web 应用](../app-service/app-service-web-overview.md)。 下面介绍如何：

> [!div class="checklist"]
> * 创建密钥保管库。
> * 在密钥保管库中存储机密。
> * 从密钥保管库检索机密。
> * 创建一个 Azure 虚拟机。
> * 为虚拟机启用[托管标识](../active-directory/managed-identities-azure-resources/overview.md)。
> * 授予所需的权限，让控制台应用程序从密钥保管库读取数据。
> * 从 Key Vault 检索机密

在我们进一步讨论之前，请阅读[基本概念](key-vault-whatis.md#basic-concepts)。

## <a name="prerequisites"></a>先决条件
* 所有平台：
  * Git（[下载](https://git-scm.com/downloads)）。
  * Azure 订阅。 如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 或更高版本。 适用于 Windows、Mac 和 Linux。

本教程使用托管服务标识

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>什么是托管服务标识？其工作原理是什么？
在进一步讨论之前，让我们了解 MSI。 Azure Key Vault 可以安全地存储凭据，因此不需将凭据置于代码中，但若要检索这些凭据，需向 Azure Key Vault 进行身份验证。 若要向 Key Vault 进行身份验证，需提供凭据！ 经典的启动问题。 通过 Azure 和 Azure AD，MSI 提供一个“启动标识”，可以大为简化启动过程。

工作方式如下！ 为 Azure 服务（例如虚拟机、应用服务或 Functions）启用 MSI 时，Azure 会为 Azure Active Directory 中的服务实例创建一个[服务主体](key-vault-whatis.md#basic-concepts)，并将服务主体的凭据注入服务实例中。 

![MSI](media/MSI.png)

接下来，代码会调用 Azure 资源上提供的本地元数据服务，以便获取访问令牌。
代码使用从本地 MSI_ENDPOINT 获取的访问令牌，以便向 Azure Key Vault 服务进行身份验证。 

## <a name="log-in-to-azure"></a>登录 Azure

若要使用 Azure CLI 登录到 Azure，请输入：

```azurecli
az login
```

## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/azure/group#az-group-create) 命令创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。

选择一个资源组名称，然后将其填充在占位符中。
以下示例在“美国西部”位置创建一个资源组：

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

刚刚创建的资源组将在整篇文章中使用。

## <a name="create-a-key-vault"></a>创建 key vault

接下来，在上一步骤创建的资源组中创建密钥保管库。 提供以下信息：

* 密钥保管库名称：名称必须为 3-24 个字符的字符串，并且只能包含 0-9、a-z、A-Z 和 -。
* 资源组名称。
* 位置：**美国西部**。

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
目前，只有你的 Azure 帐户才有权对这个新保管库执行任何操作。

## <a name="add-a-secret-to-the-key-vault"></a>向密钥保管库添加机密

我们将添加机密以帮助说明这是如何工作的。 可以存储需要安全保存的，但同时也要提供给应用程序使用的 SQL 连接字符串或其他任何信息。

键入以下命令，在名为 **AppSecret** 的密钥保管库中创建机密。 此机密将存储值“MySecret”。

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>创建虚拟机
按照以下链接的说明创建 Windows 虚拟机

[Azure CLI](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli) 

[Powershell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)

[门户](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-identity-to-virtual-machine"></a>为虚拟机分配标识
在此步骤中，我们将为虚拟机创建一个系统分配标识，方法是在 Azure CLI 中运行以下命令

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

请记下下面显示的 systemAssignedIdentity。 以上命令的输出将为 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-virtual-machine-identity-permission-to-key-vault"></a>为虚拟机标识提供 Key Vault 访问权限
现在，我们可以运行以下命令，为上面创建的标识授予 Key Vault 访问权限

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>登录到虚拟机

可以按此[教程](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)的说明操作

## <a name="create-and-run-sample-python-app"></a>创建并运行示例 Python 应用

下面是一个名为“Sample.py”的示例文件。 它使用 [requests](http://docs.python-requests.org/master/) 库进行 HTTP GET 调用。

## <a name="edit-samplepy"></a>编辑 Sample.py
创建 Sample.py 后，打开该文件并复制下面的代码

```
The below is a 2 step process. 
1. Fetch a token from the local MSI endpoint on the VM which in turn fetches a token from Azure Active Directory
2. Pass the token to Key Vault and fetch your secret 
```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets a access_token from Azure Active Directory using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to Key Vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

By running you should see the secret value 
```
python Sample.py
```

The above code shows you how to do operations with Azure Key Vault in an Azure Windows Virtual Machine. 

## Next steps

> [!div class="nextstepaction"]
> [Azure Key Vault REST API](https://docs.microsoft.com/rest/api/keyvault/)
