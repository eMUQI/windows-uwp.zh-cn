---
title: / untrustedplatform/用户/批处理/scid / {scid} /data/ {pathAndFileName} {类型}
assetID: c2909e75-e108-3555-c157-f2d552f10e9f
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " / untrustedplatform/用户/批处理/scid / {scid} /data/ {pathAndFileName} {类型}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 04952a183a2903a5c296a6b27af35766bccbddc1
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3880956"
---
# <a name="untrustedplatformusersbatchscidssciddatapathandfilenametype"></a>/ untrustedplatform/用户/批处理/scid / {scid} /data/ {pathAndFileName} {类型}
将多个文件下载从多个用户具有相同的文件名。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| scid| guid| 若要查找的服务配置 ID。| 
| pathAndFileName| 字符串| 若要访问该项目的路径和文件名称。 有效的字符 （甚至最终正斜杠） 的路径部分包含大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)，下划线 (_) 和正斜杠 （/）。路径部分可能为空。有效的字符 （所有最终正斜杠后） 的文件名称部分包含大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9) 的下划线 (_)，句点 （.） 和连字符 （-）。 文件名称不能为空，以句号结尾或包含两个连续句点。| 
| type| 字符串| 数据的格式。 可能的值为二进制文件的 json。| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>有效的方法

[POST](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;将多个文件下载从多个用户具有相同的文件名。
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[标题存储 Uri](atoc-reference-storagev2.md)

   