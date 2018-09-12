---
title: 发布/用户/xuid (xuid) / 列出了/PIN / {listname} / 索引 （{索引}）？ insertIndex = {insertIndex}
assetID: df61be42-c229-7408-5e4c-dbf4ae95b52b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindexpost.html
author: KevinAsgari
description: " 发布/用户/xuid (xuid) / 列出了/PIN / {listname} / 索引 （{索引}）？ insertIndex = {insertIndex}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1aecb7f73a49e7628b076fe943774ccf89aa71bc
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3880890"
---
# <a name="post-usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>发布/用户/xuid (xuid) / 列出了/PIN / {listname} / 索引 （{索引}）？ insertIndex = {insertIndex}
将列表中的项移到列表中的不同位置。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EEB)
  * [查询字符串参数](#ID4EWC)
  * [请求正文](#ID4EVD)
  * [HTTP 状态代码](#ID4EEE)
  * [需的请求标头](#ID4E1BAC)
  * [响应正文](#ID4EQDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注 
 
提供此调用以允许客户端能够轻松地将项目移至单个操作中的列表中的不同索引。 可以一次移动只有一个项。 如果项目要移动的索引不存在，则将返回 HTTP 400。 该插入点的索引遵循标准插入相同的规则。 它将默认为 0-列表的开头和任何大于列表中的项目数重新插入列表末尾处的项。 同样可以通过 insertIndex"端"指示列表末尾。 
 
此调用还允许你确定要移动的 itemId （或提供程序/providerId 组合） 的项。 在此情况下，必须在请求正文中传递 itemId，并且它必须匹配列表中的现有项目。 如果不存在，HTTP 400 错误中将返回与 IndexOutOfRange 描述;对于在调用此特殊版本，使用"-1"作为项目的索引移动。 
 
此调用需要"如果-匹配： versionNumber"标头包含在请求如果指定项目的索引。 如果使用 itemId 来指示要移动的项目，然后"If-match"标头是可选的。 如果存在，将始终验证"If-match"标头。 在"If-match"标头，versionNumber 是列表的当前版本号。 如果它不包含 （和所需），或不匹配当前将返回号，则 HTTP 412 – 前置条件失败的状态代码的列表版本，并且该响应正文将包含最新的元数据的列表，其中包括当前版本号. 这是为了防止从不同的客户端上彼此造成破坏的更新。 
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>URI 参数 
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| XUID| 字符串| 用户的 XUID。| 
| listname| 字符串| 列表来操作的名称。| 
| 索引| 字符串| 指定要移动的项的当前索引。 零个或正整数索引值时，这是指项的当前的索引和调用的请求正文应为空。 但是，如果索引值为"-1"，必须由 ItemId 或提供程序/ProviderID 调用，请求正文中指定要移动的项。| 
  
<a id="ID4EWC"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数 
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| 字符串| 指定要插入项的列表位置。 允许的值为零，正整数和"结束"。 "结束"将该项目放置在当前列表末尾。 如果指定的值列表末尾超出，该项目插入到列表末尾。 | 
  
<a id="ID4EVD"></a>

 
## <a name="request-body"></a>请求正文 
 
指定项时，仅发送请求正文，以通过 itemId 或通过提供程序/ProviderId 移动。
 
<a id="ID4E6D"></a>

  
示例请求 

```cpp
{
  "Item":
  {
    "ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }
}
    
```

  
<a id="ID4EEE"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码 
 
该服务将返回一个状态代码此部分中使用此方法对此资源进行的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定” | 请求已成功完成。 响应正文应包含所请求的资源 （GET)。 POST 和 PUT 请求将接收最新列表元数据 （列表版本、 计数等）。| 
| 201| 已创建 | 已创建新的列表。 这被返回初始插入到列表。 该响应包括在列表上保持最新的元数据和位置标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端具有最新版本的列表。 该服务将列表版本<b>If-match</b>标头中的值进行比较。 如果相等，然后 304 获取不返回任何数据。 | 
| 400| 错误请求 | 请求已格式不正确。 可能的值无效或 URI 或查询字符串参数类型。 也可以是缺少所需的参数的结果或数据值或请求 API 的丢失或无效的版本。 请参阅<b>X XBL 协定版本</b>标头。 | 
| 401| 未授权 | 请求要求用户身份验证。| 
| 403| 已禁止 | 为用户或服务不允许该请求。| 
| 404| 找不到 | 调用方没有访问资源。 这指示已创建了任何此类列表。| 
| 412| 前置条件失败 | 指示已更改的列表版本和修改请求失败。 修改请求是插入、 更新或删除。 服务会检查列表版本的<b>If-match</b>标头文件。 如果它不匹配当前版本的列表，然后操作将失败，这会返回以及当前列表元数据 （其中包括当前版本）。 | 
| 500| 内部服务器错误 | 该服务拒绝由于服务器端错误请求。| 
| 501| 未实现 | 调用方请求尚未实现在服务器的 URI。 （当前仅时请求使用由列表名称不是列入白名单。）| 
| 503| 服务不可用 | 服务器拒绝请求，通常由于过多的负载。 延迟后 (请参阅<b>retry-after</b>标头)，可以重试请求。 | 
  
<a id="ID4E1BAC"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于身份验证和授权请求的 STS 标记。 必须从 XSTS 服务令牌并包括为一个声明的 XUID。 | 
| X XBL 协定版本| 指定哪些 API 版本正在请求 （正整数）。 引脚支持版本 2。 如果此标头丢失或不受支持值，则服务将返回 400 – 错误请求与"不受支持或丢失合约版本标头"状态描述中。| 
| Content-Type| 指定内容的请求/响应正文将包含在 json 或 xml。 受支持的值为"应用程序/json"和"应用程序/xml"| 
| If-Match| 进行修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT、 POST，或删除动词命令。 如果"If-match"标头中的版本不匹配当前版本的列表，将拒绝请求，并 HTTP 412 – 前置条件失败返回代码。 （可选）此外可为获取，如果存在并传入的版本匹配当前的列表版本，然后没有列表数据和 HTTP 304 – 将返回不修改代码。 | 
  
<a id="ID4EQDAC"></a>

 
## <a name="response-body"></a>响应正文 
 
如果在调用成功，该服务返回的最新的元数据的列表。 
 
<a id="ID4E1DAC"></a>

 
### <a name="sample-response"></a>示例响应 
 

```cpp
{ 
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}

      
```

   
<a id="ID4EIEAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EKEAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/ 用户/xuid (xuid) / 列出了/PIN / {listname} / 索引 （{索引}）？ insertIndex = {insertIndex}](uri-usersxuidlistspinslistnameindex.md)

   