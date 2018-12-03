---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 7e390e2134df02b0ca9c0d1752c3207aff7b9314
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279460"
---
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 每个云服务的[虚拟机数](../articles/virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)<sup>1</sup> |50 |50 |
| 每个云服务的输入终结点数<sup>2</sup> |150 |150 |

<sup>1</sup>在服务管理（而不是 Resource Manager ）中创建的虚拟机会自动存储在云服务中。 可以向该云服务添加更多虚拟机以获得负载均衡和可用性。 

<sup>2</sup>输入终结点允许从某个虚拟机的云服务外部与该虚拟机通信。 位于同一云服务或虚拟网络中的虚拟机可以自动相互通信。 请参阅[如何设置虚拟机的终结点](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)。 
