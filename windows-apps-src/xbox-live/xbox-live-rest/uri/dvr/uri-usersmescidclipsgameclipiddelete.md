---
title: 删除 (/ 用户/me/scid / {scid} /clips/ {gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
author: KevinAsgari
description: " 删除 (/ 用户/me/scid / {scid} /clips/ {gameClipId})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a68d765cfdec81da064b0522ea2ff9a4be12bafb
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3881012"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>删除 (/ 用户/me/scid / {scid} /clips/ {gameClipId})
删除这些 Uri 的域的游戏剪辑`gameclipsmetadata.xboxlive.com`和`gameclipstransfer.xboxlive.com`根据问题的 URI 的函数。
 
  * [备注](#ID4EX)
  * [URI 参数](#ID4ECB)
  * [授权](#ID4ENB)
  * [需的请求标头](#ID4EYB)
  * [可选的请求标头](#ID4EEE)
  * [请求正文](#ID4ENF)
  * [HTTP 状态代码](#ID4EYF)
  * [所需的响应标头](#ID4EIAAC)
  * [可选的响应标头](#ID4E2CAC)
  * [响应正文](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>备注
 
提供用于 GameClips 服务中删除用户的视频的机制。 在删除后从系统删除所有元数据和实际的视频资产 （生成和原始）。 这是永久操作。 

> [!NOTE] 
> 指定的所有者 ID 必须匹配中删除请求成功的授权令牌的调用方。 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | 
| scid| 字符串| 所访问的资源的服务配置 ID。 必须匹配的身份验证的用户的 SCID。| 
| gameClipId| 字符串| GameClip 所访问的资源的 ID。| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>授权
 
对于此方法需要仅 Xuid 声明。
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | 
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <b>Xauth =&lt;authtoken ></b>| 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证该标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>可选的请求标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| 字符串| 可接受的压缩编码。 示例值： gzip，桥，标识。| 
| ETag| 字符串| 用于缓存优化。 示例值:"686897696a7c876b7e"。| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
该服务将返回一个状态代码此部分中使用此方法对此资源进行的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| “确定”| 该剪辑的成功删除。| 
| 401| 未授权| 没有问题在请求中的身份验证令牌格式。| 
| 403| 已禁止| 缺少某些必需声明。| 
| 404| 找不到| 在 URL 中指定该剪辑时不存在 （或已被删除第二次）。| 
| 503| 不允许| 该服务或一些下游的依赖项都已关闭。 使用标准后关闭行为重试。| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>所需的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/数。 验证该标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例： 1，vnext。| 
| Content-Type| 字符串| 响应正文的 MIME 类型。 示例：<b>应用程序/json</b>。| 
| 缓存控制| 字符串| 若要指定缓存行为的礼貌请求。| 
| 接受| 字符串| 内容类型的可接受的值。 示例：<b>应用程序/json</b>。| 
| 重试后| 字符串| 指示客户端在不可用的服务器的情况下稍后重试。| 
| 不同| 字符串| 指示下游代理如何缓存响应。| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>可选的响应标头
 
| 标头| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Etag| 字符串| 用于缓存优化。 示例:"686897696a7c876b7e"。| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>响应正文
 
该服务将通过 HTTP 状态代码的 204 （任何内容） 后成功做出响应。 尝试为删除同一个对象，或不存在对象将返回 404。
 
发生错误，将返回一个**ServiceErrorResponse**对象。
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/ 用户/me/scid / {scid} /clips/ {gameClipId}](uri-usersmescidclipsgameclipid.md)

   