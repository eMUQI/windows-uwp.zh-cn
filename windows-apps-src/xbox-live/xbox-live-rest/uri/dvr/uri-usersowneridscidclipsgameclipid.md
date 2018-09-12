---
title: /users/ {ownerId} {scid} /scids/ /clips/ {gameClipId}
assetID: 49b68418-71f1-c5a2-3a9b-869fd1fa663c
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipid.html
author: KevinAsgari
description: " /users/ {ownerId} {scid} /scids/ /clips/ {gameClipId}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d12f9e530ee6d703aa324cb6380591aab31facfd
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881088"
---
# <a name="usersowneridscidsscidclipsgameclipid"></a>/users/ {ownerId} {scid} /scids/ /clips/ {gameClipId}
如果已知的所有 Id，以找到它，请从系统中访问的单个游戏剪辑。 这些 Uri 的域是`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`根据问题的 URI 的函数。
 
  * [URI 参数](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| ownerId| 字符串| 用户的用户身份的所访问的资源。 支持的格式:"我"或"xuid(123456789)"。 最大长度： 16。| 
| scid| 字符串| 所访问的资源的服务配置 ID。 必须匹配的身份验证的用户的 SCID。| 
| gameClipId| 字符串| GameClip 所访问的资源的 ID。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[获取 (/users/ {ownerId} {scid} /scids/ /clips/ {gameClipId})](uri-usersowneridscidclipsgameclipidget.md)

&nbsp;&nbsp;如果已知的所有 Id，以找到它，请从系统中获取单个游戏剪辑。
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[游戏 DVR Uri](atoc-reference-dvr.md)

   