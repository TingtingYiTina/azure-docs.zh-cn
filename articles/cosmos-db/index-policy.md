---
title: Azure Cosmos DB 索引策略 | Microsoft Docs
description: 了解 Azure Cosmos DB 中索引的工作原理。 了解如何配置和更改索引策略，实现自动索引并提高性能。
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: ffb70ce8c26b7774e90801271c55cd8a80906c90
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628336"
---
# <a name="indexing-policy-in-azure-cosmos-db"></a>Azure Cosmos DB 中的索引策略

可以通过配置以下参数来替代 Azure Cosmos 容器上的默认索引策略：

* **Include or exclude items and paths from the index**：在插入或替换容器中的项时，可以在索引中排除或包含特定项。 还可以包含或排除要跨容器编制索引的特定路径/属性。 路径可能包括通配符模式，例如 *。

* **Configure index types**：除范围索引路径外，还可以添加其他类型的索引，例如空间。

* **Configure index modes**：通过在容器上使用索引策略，可以配置不同的索引模式，例如“一致”或“无”。

## <a name="indexing-modes"></a>索引模式 

Azure Cosmos DB 支持两种索引模式，可以在 Azure Cosmos 容器上配置这些模式。 可以通过索引策略配置以下两种索引模式： 

* **一致**：如果 Azure Cosmos 容器的策略设置为“一致”，则特定容器上的查询将按照为点读取指定的一致性级别进行（例如，强、有限过期性、会话或最终）。 

  更新项时，会同步更新索引。 例如，对项执行插入、替换、更新和删除操作将导致更新索引。 一致索引支持一致的查询，但代价是影响写入吞吐量。 写入吞吐量的降低取决于“索引中包含的路径”和“一致性级别”。 一致的索引模式适用于“快速写入和立即查询”的工作负荷。

* **无**：索引模式为“无”的容器没有与之关联的索引。 如果 Azure Cosmos 数据库用作键值存储，并且只通过项的 ID 属性访问项，则通常使用该模式。

  > [!NOTE]
  > 将索引模式配置为“无”时，删除任何现有索引会产生不良影响。 如果访问模式仅需要 ID 或自助链接，则应使用此选项。

查询一致性级别的维护与常规读取操作的维护类似。 如果查询具有“无”索引模式的容器，Azure Cosmos 数据库将返回错误。 可使用 .NET SDK 通过 REST API 中的显式  **x-ms-documentdb-enable-scan** 标头或  **EnableScanInQuery** 请求选项将查询作为扫描执行。  “EnableScanInQuery”当前不支持某些查询功能（如 ORDER BY），因为这些功能授权相应索引。

## <a name="modifying-the-indexing-policy"></a>修改索引策略

在 Azure Cosmos DB 中，可以随时更新容器的索引策略。 Azure Cosmos 容器上索引策略的更改可能会导致索引形状的更改。 此更改会影响可编制索引的路径、其精度以及索引本身的一致性模型。 索引策略的更改实际上要求将旧索引转换为新索引。

### <a name="index-transformations"></a>索引转换

所有索引转换都是联机进行的。 按照旧策略编制索引的项可以按照新策略有效转换，而不会影响写入可用性或为容器预配的吞吐量。 在索引转换期间，使用 REST API、SDK 或在存储过程和触发器中执行的读取和写入操作的一致性不会受到影响。

更改索引策略是一种异步操作，完成操作的时间取决于项数、预配的吞吐量和项大小。 在重新编制索引的过程中，如果查询碰巧使用正在修改的索引，则查询可能不会返回所有匹配的结果且不会返回任何错误/失败。 虽然正在重新编制索引，但是，无论索引模式配置如何，期间查询始终一致。 索引转换完成后，将继续看到一致的结果。 这适用于由 REST API、SDK 等接口或存储过程和触发器发出的查询。 使用特定副本可用的备用资源在后台以异步方式对副本执行索引转换。

所有索引转换都已到位。 Azure Cosmos DB 不维护索引的两个副本。 因此，在索引转换发生时，容器中不需要也不占用额外的磁盘空间。

更改索引策略时，更改得到应用，主要基于索引模式配置从旧索引移动到新索引。 与其他属性（如包括/排除路径，索引种类和精度）相比，索引模式配置起主要作用。

如果新旧索引策略都使用“一致”索引，则 Azure Cosmos 数据库执行联机索引转换。 转换正在进行时，不能应用一致索引模式下的其他索引策略更改。 当切换到“无”索引模式时，会立即删除索引。 当想要取消正在进行的转换，并开始使用不同的索引策略时，切换到“无”模式非常有用。

要成功完成索引转换，请确保容器具有足够的存储空间。 如果容器达到其存储配额，会暂停索引转换。 获得可用的存储空间后（例如删除某些项），索引转换自动恢复。

## <a name="modifying-the-indexing-policy---examples"></a>修改索引策略 - 示例

以下是更新索引策略的最常见用例：

* 如果要在正常操作期间获得一致的结果，但在批量数据导入期间回退到“无”索引模式。

* 如果要在当前 Azure Cosmos 容器上开始使用索引功能。 例如，可使用地理空间查询，这需要空间索引种类；或使用 ORDER BY/字符串范围查询，这需要字符串范围索引种类。

* 如果要手动选择要编制索引的属性，并随时更改它们以适应工作负载。

* 如果要调整索引精度，以提高查询性能或减少占用的存储。

## <a name="next-steps"></a>后续步骤

阅读以下文章中有关索引的详细信息：

* [索引概览](index-overview.md)
* [索引类型](index-types.md)
* [索引路径](index-paths.md)
* [如何管理索引策略](how-to-manage-indexing-policy.md)