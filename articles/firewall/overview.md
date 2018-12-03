---
title: 什么是 Azure 防火墙？
description: 了解 Azure 防火墙功能。
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 11/28/2018
ms.author: victorh
ms.openlocfilehash: b90496b0ccc6c8243c2d1b3ead1e7c4faa4801ec
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582032"
---
# <a name="what-is-azure-firewall"></a>什么是 Azure 防火墙？

Azure 防火墙是托管的基于云的网络安全服务，可保护 Azure 虚拟网络资源。 它是一个服务形式的完全有状态防火墙，具有内置的高可用性和不受限制的云可伸缩性。 

![防火墙概述](media/overview/firewall-overview.png)

可以跨订阅和虚拟网络集中创建、实施和记录应用程序与网络连接策略。 Azure 防火墙对虚拟网络资源使用静态公共 IP 地址，使外部防火墙能够识别来自你的虚拟网络的流量。  该服务与用于日志记录和分析的 Azure Monitor 完全集成。

## <a name="features"></a>功能

Azure 防火墙提供以下功能：

### <a name="built-in-high-availability"></a>内置的高可用性
内置高可用性，因此不需要部署额外的负载均衡器，也不需要进行任何配置。

### <a name="unrestricted-cloud-scalability"></a>不受限制的云可伸缩性 
为了适应不断变化的网络流量流，Azure 防火墙可尽最大程度进行纵向扩展，因此不需要为峰值流量做出预算。

### <a name="application-fqdn-filtering-rules"></a>应用程序 FQDN 筛选规则

可将出站 HTTP/S 流量限制为指定的完全限定域名 (FQDN) 列表，包括通配符域名。 此功能不需要 SSL 终止。

### <a name="network-traffic-filtering-rules"></a>网络流量筛选规则

可以根据源和目标 IP 地址、端口和协议，集中创建“允许”或“拒绝”网络筛选规则。 Azure 防火墙是完全有状态的，因此它能区分不同类型的连接的合法数据包。 将跨多个订阅和虚拟网络实施与记录规则。

### <a name="fqdn-tags"></a>FQDN 标记

FQDN 标记使你可以轻松地允许已知的 Azure 服务网络流量通过防火墙。 例如，假设你想要允许 Windows 更新网络流量通过防火墙。 创建应用程序规则，并在其中包括 Windows 更新标记。 现在，来自 Windows 更新的网络流量将可以流经防火墙。

### <a name="outbound-snat-support"></a>出站 SNAT 支持

所有出站虚拟网络流量 IP 地址将转换为 Azure 防火墙公共 IP（源网络地址转换）。 可以识别源自你的虚拟网络的流量，并允许将其发往远程 Internet 目标。

### <a name="inbound-dnat-support"></a>入站 DNAT 支持

转换到防火墙公共 IP 地址的入站网络流量（目标网络地址转换）并将其筛选到虚拟网络上的专用 IP 地址。 

### <a name="azure-monitor-logging"></a>Azure Monitor 日志记录

所有事件与 Azure Monitor 集成，使你能够在存储帐户中存档日志、将事件流式传输到事件中心，或者将其发送到 Log Analytics。

## <a name="known-issues"></a>已知问题

Azure 防火墙存在以下已知问题：


|问题  |Description  |缓解措施  |
|---------|---------|---------|
|与 Azure 安全中心 (ASC) 实时 (JIT) 功能冲突|如果使用 JIT 访问虚拟机，并且虚拟机位于具有用户定义路由的子网中，而该路由指向用作默认网关的 Azure 防火墙，则 ASC JIT 不起作用。 这种结果是非对称路由造成的 – 数据包通过虚拟机公共 IP 传入（JIT 开放了访问权限），但返回路径是通过防火墙形成的，因此丢弃了数据包，因为防火墙上未建立会话。|若要解决此问题，请将 JIT 虚拟机放置在未与防火墙建立用户定义的路由的独立子网中。|
|不支持使用全局对等互连的中心辐射模型|使用中心辐射模型时，中心和防火墙部署在一个 Azure 区域，分支部署在另一个 Azure 区域。 不支持通过全局 VNet 对等互连连接到中心。|这是设计使然。 有关详细信息，请参阅 [Azure 订阅和服务限制、配额与约束](../azure-subscription-service-limits.md#azure-firewall-limits)|
针对 TCP/UDP 协议（例如 ICMP）的网络筛选规则不适用于 Internet 绑定的流量|针对非 TCP/UDP 协议的网络筛选规则不支持公共 IP 地址的 SNAT。 在分支子网与 VNet 之间支持非 TCP/UDP 协议。|Azure 防火墙使用[目前不支持 IP 协议 SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#limitations) 的标准负载均衡器。 我们正在探索如何在将来的版本中推出支持此方案的选项。|
|缺少对 ICMP 的 PowerShell 和 CLI 支持|Azure PowerShell 和 CLI 不支持使用 ICMP 作为网络规则中的有效协议。|仍然可以通过门户和 REST API 使用 ICMP 作为协议。 我们正在致力于在不久之后在 PowerShell 和 CLI 中添加 ICMP。|
|FQDN 标记要求设置 protocol: port|带有 FQDN 标记的应用程序规则需要 port:protocol 定义。|可以将 **https** 用作 port: protocol 值。 我们正在致力于使此字段在使用了 FQDN 标记时可选。|
|不支持将防火墙移动到不同的资源组或订阅。|不支持将防火墙移动到不同的资源组或订阅。|我们已计划提供此功能的支持。 若要将防火墙移动到不同的资源组或订阅，必须删除当前实例并在新的资源组或订阅中重新创建它。|

## <a name="next-steps"></a>后续步骤

- [教程：使用 Azure 门户部署和配置 Azure 防火墙](tutorial-firewall-deploy-portal.md)
- [使用模板部署 Azure 防火墙](deploy-template.md)
- [创建 Azure 防火墙测试环境](scripts/sample-create-firewall-test.md)

