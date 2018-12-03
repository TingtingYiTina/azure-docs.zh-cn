---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 2b6ba1dcddd435c42ad864b8e87175d0e98c9b3a
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279470"
---
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| [每个部署的 Web/辅助角色数](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| 每个部署的[实例输入终结点数](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) |25 |25 |
| 每个部署的[输入终结点数](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) |25 |25 |
| 每个部署的[内部终结点数](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) |25 |25 |

<sup>1</sup>包含 Web/辅助角色的每个云服务可以有两个部署，一个用于生产，一个用于过渡。 另请注意，此限制与不同的角色数目（配置）相关，而不是与每个角色的实例数目（缩放）相关。

