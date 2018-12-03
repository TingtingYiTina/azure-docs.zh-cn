---
title: 查看和编辑视频索引器见解
titlesuffix: Azure Media Services
description: 本主题演示如何查看和编辑视频索引器见解。
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: a8b80c02da148d18daf39fb6196ebcb388c96ee4
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291739"
---
# <a name="view-and-edit-video-indexer-insights"></a>查看和编辑视频索引器见解

本主题介绍如何查看和编辑视频的视频索引器见解。

1. 浏览到[视频索引器](https://www.videoindexer.ai/)网站并登录。
2. 找到需从其创建视频索引器见解的视频。 有关详细信息，请参阅[在视频中查找确切的时刻](video-indexer-search.md)。
3. 按“播放”。

    页面会显示视频的汇总见解。 

    ![洞察力](./media/video-indexer-view-edit/video-indexer-summarized-insights.png)

4. 查看视频的汇总见解。 

    汇总见解显示聚合视图形式的数据：人脸、关键字、情绪。 例如，可以看到人脸和每个人脸出现的时间范围，以及显示人脸的时间百分比。

    播放器和见解是同步的。 例如，如果单击某个关键字或脚本行，播放器会将你带到视频中的相应时刻。 可以在应用程序中获得播放器/见解视图和同步。 有关详细信息，请参阅[将 Azure 索引器小组件嵌入应用程序](video-indexer-embed-widgets.md)。 

3. 编辑视频索引器见解。

    按视频下的“编辑”。 此时会显示一个页面，其中会显示视频的所有明细。 明细会分成块。 有了块，就可以更轻松地浏览数据。 例如，可以在发言人换人或中断时间长时将块细分。 可以创建你自己的播放列表，其中只包含所需的行。 若要只显示源视频的特定部分，可以按主题/关键字、情绪、人物、发言人进行筛选。 可以选择只查看视频的脚本或 OCR。  

    ![洞察力](./media/video-indexer-view-edit/video-indexer-create-new-playlist.png)

## <a name="next-steps"></a>后续步骤

[了解如何根据一些其他的视频创建你自己的视频索引器见解](video-indexer-create-new.md)。

## <a name="see-also"></a>另请参阅

[视频索引器概述](video-indexer-overview.md)

