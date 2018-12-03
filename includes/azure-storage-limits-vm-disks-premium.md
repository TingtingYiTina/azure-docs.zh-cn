---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: 5ac7982d306125804fc5b7873e537f9381f717cb
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279471"
---
**高级非托管虚拟机磁盘：每个帐户的限制**

| 资源 | 默认限制 |
| --- | --- |
| 每个帐户的总磁盘容量 |35 TB |
| 每个帐户的总快照容量 |10 TB |
| 每个帐户的最大带宽（传入 + 传出<sup>1</sup>） |<=50 Gbps |

<sup>1</sup>“传入”是指发送到存储帐户的所有数据（请求）。 “传出”是指从存储帐户接收的所有数据（响应）。

**高级非托管虚拟机磁盘：每个磁盘的限制**

| 高级存储磁盘类型 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| 磁盘大小 |128 GiB |512 GiB |1024 GiB (1 TB) |2048 GiB (2 TB)|4095 GiB (4 TB)|
| 每个磁盘的最大 IOPS |500 |2300 |5000 |7500 |7500 |
| 每个磁盘的最大吞吐量 |100 MB/秒 | 150 MB/秒 |200 MB/秒 |250 MB/秒 |250 MB/秒 |
| 每个存储帐户的磁盘的最大数目 |280 |70 |35 | 17 | 8 |

**高级非托管虚拟机磁盘：每个 VM 的限制**

| 资源 | 默认限制 |
| --- | --- |
| 每个 VM 的最大 IOPS |GS5 VM 为 80,000 IOPS |
| 每个 VM 的最大吞吐量 |GS5 VM 为 2,000 MB/秒 |

