---
title: 如何使用 Azure Cloud Shell 创建为 Azure Dev Spaces 启用的 Kubernetes 群集 | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: article
description: 了解如何直接从浏览器快速创建为 Azure Dev Spaces 启用的 Kubernetes 群集，而无需安装任何内容。
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes 服务, 容器
ms.openlocfilehash: 61a466efdacb56889a7e437d84d3c9de43927e65
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705746"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>使用 Azure Cloud Shell 创建 Kubernetes 群集

可以使用 [Azure Cloud Shell](/azure/cloud-shell) 通过使用此页中的“试用”按钮为 Azure Dev Spaces 创建群集。 如果未登录，请按照提示使用 Azure 帐户登录，然后在出现 Azure Cloud Shell 提示符时在 Azure Cloud Shell 提示符下键入命令。

## <a name="create-the-cluster"></a>创建群集

首先，创建资源组。 使用当前支持的区域（EastUS、EastUS2、CentralUS、WestUS2、WestEurope、SoutheastAsia、CanadaCentral 或 CanadaEast）之一。

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

使用以下命令创建 Kubernetes 群集：

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.3 --enable-addons http_application_routing
```

创建群集需要几分钟时间。  完成后，输出以 JSON 格式显示。 查找 `provisioningState` 并验证它是否为 `Succeeded`。

## <a name="next-steps"></a>后续步骤

如需完整教程的链接，请参阅 [Azure Dev Spaces](/azure/dev-spaces/)。
