---
title: TitleRecord (JSON)
assetID: 8e1bd699-e408-67c8-31da-2d968adfbc21
permalink: en-us/docs/xboxlive/rest/json-titlerecord.html
author: KevinAsgari
description: " TitleRecord (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4e7fb10a0f81e24215ebc24d2545f1197d4520bc
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881026"
---
# <a name="titlerecord-json"></a>TitleRecord (JSON)
有关游戏，包括其名称和上次修改时间戳的信息。 
<a id="ID4EN"></a>

 
## <a name="titlerecord"></a>TitleRecord
 
TitleRecord 必须包含 DeviceRecord 或 LastSeenRecord，但不能包含两者。
 
TitleRecord 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| id| 32 位无符号的整数| 职务记录的 Id。| 
| name| 字符串| 本地化的游戏的名称。| 
| 活动| [ActivityRecord](json-activityrecord.md)| 在游戏中的用户的活动。 仅深度为"所有"时返回。| 
| lastModified| DateTime| UTC 时间戳记录上次更新时。| 
| 放置| 字符串| 用户界面中应用的位置。 可能性包括"fill"、"完全"、"贴靠时，"或"后台"。 默认值为"完全"，而无需将应用的功能的设备。| 
| 状态| 字符串| 游戏的状态。 可以是"活动"或"非活动"（默认值）。 标题设置基于其自己的标准的活动和非活动状态的状态。| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    }
    
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUD"></a>

 
##### <a name="reference"></a>参考 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   