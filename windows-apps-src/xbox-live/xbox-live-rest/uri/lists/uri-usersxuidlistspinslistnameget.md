---
title: GET (/users/xuid(xuid)/lists/PINS/{listname})
assetID: a63f595a-61dd-5885-c405-9833230abb94
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameget.html
author: KevinAsgari
description: " GET (/users/xuid(xuid)/lists/PINS/{listname})"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 42f40428f79110e55aae7e881c25a3789899fee7
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3880437"
---
# <a name="get-usersxuidxuidlistspinslistname"></a>GET (/users/xuid(xuid)/lists/PINS/{listname})
返回列表的内容。 这些 Uri 的域是`eplists.xboxlive.com`。
 
  * [备注](#ID4EV)
  * [URI 参数](#ID4EIB)
  * [查询字符串参数](#ID4ETB)
  * [授权](#ID4ESD)
  * [需的请求标头](#ID4E6D)
  * [请求正文](#ID4EVF)
  * [HTTP 状态代码](#ID4EAG)
  * [响应正文](#ID4E5CAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>备注
 
中返回的数据的**listCount**字段表示多少项位于维护服务-此类的总列表，它可以用于确定其中列表末尾，并且这可能是数不同数量的特定项已 retu该请求 rned。
 
如果列表尚不存在，则结果将包含列表项、 **listCount**将为零， **listVersion**将为零。
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| xuid| 字符串| Xbox 用户 ID (XUID)。| 
| listtype| 字符串| 列表 （如何使用和其工作原理） 的类型。 始终"固定"这些相关的方法。| 
| listname| 字符串| 列表的名称 （给定 listtype 执行哪些列表）。 始终为"XBLPins"Pin 中的项。| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>查询字符串参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| 32 位有符号整数| 可选。 若要在返回结果之前枚举中跳过的项目数。 默认值： 0。| 
| maxItems| 32 位有符号整数| 可选。 要返回的项目的最大数量。 如果请求中指定没有最大值，默认值为 25 个项目。 该服务不会将最多入此值。如果值大于列表中的项目数，然后将并不报错返回所有项。| 
| filterItemId| 字符串| 可选。 指定要在列表中找到的项。 返回列表中的项的所有实例。 允许客户端来快速确定是否以及所在列表中的项。 方便的大型列表，而无需通过将整个列表进行迭代，来确定某个项目的所有实例。 默认值： null。| 
| filterContentType| 字符串| 可选。 指定要返回的内容类型的以逗号分隔列表 （将不会返回类型不在列表中）。 用于仅从列表中获取某些内容的类型。 此筛选器使用逗号分隔的内容类型列表。 （多个内容类型可通过查询中调用一次。）受支持的内容类型包括定义娱乐发现服务 (EDS) 的所有媒体类型。 默认值： null （所有内容类型）。| 
| filterDeviceType| 字符串| 可选。 指定要返回的设备类型的以逗号分隔列表 （将不会返回类型不在列表中）。 筛选器返回的设置为仅返回已插入从一组特定的设备类型的项。 用逗号分隔的设备类型列表用于此筛选器 （多个设备类型可查询中调用一次）。 可能的值： xbox One、 MCapensis、 WindowsPhone、 WindowsPhone7、 Web、 PC、 MoLive。 默认值： null （所有内容类型）。| 
  
<a id="ID4ESD"></a>

 
## <a name="authorization"></a>授权
 
此调用在**授权**标头中预期 SAML XSTS 令牌。 Xuid 声明必须存在于该 SAML 标记来标识调用方。 使用此值来确定调用方是否有问题的列表数据的访问权限。 列表本身的 Xuid 也将标识和将包含在列表的 URI。 使用此，我们将来可能支持共享访问列表，但不一项功能在此时间。 目前，所有用户访问的列表都将自己并且没有共享的访问。 因此在 URI 中的 Xuid 必须匹配 SAML 声明令牌中的 Xuid。 

> [!NOTE] 
> 目前支持 XBL 身份验证 2.0 和 3.0 令牌。 


  
<a id="ID4E6D"></a>

 
## <a name="required-request-headers"></a>需的请求标头
 
| 标题| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 授权| 包含用于身份验证和授权请求的 STS 标记。 必须从 XSTS 服务令牌并包括为一个声明的 XUID。 | 
| X XBL 协定版本| 指定哪些 API 版本正在请求 （正整数）。 引脚支持版本 2。 如果此标头丢失或不受支持值，则服务将返回 400 – 错误请求与"不受支持或丢失合约版本标头"状态描述中。| 
| Content-Type| 指定内容的请求/响应正文将包含在 json 或 xml。 受支持的值为"应用程序/json"和"应用程序/xml"| 
| If-Match| 进行修改请求时，此标头必须包含列表的当前版本号。 修改请求使用 PUT、 POST，或删除动词命令。 如果"If-match"标头中的版本不匹配当前版本的列表，将拒绝请求，并 HTTP 412 – 前置条件失败返回代码。 （可选）此外可为获取，如果存在并传入的版本匹配当前的列表版本，然后没有列表数据和 HTTP 304 – 将返回不修改代码。 | 
  
<a id="ID4EVF"></a>

 
## <a name="request-body"></a>请求正文
 
此请求的正文中不发送任何对象。
  
<a id="ID4EAG"></a>

 
## <a name="http-status-codes"></a>HTTP 状态代码
 
该服务将返回一个状态代码此部分中使用此方法对此资源进行的请求的响应。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。
 
| 代码| 原因短语| 说明| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| “确定”| 请求已成功完成。 响应正文应包含所请求的资源 （GET)。 POST 和 PUT 请求将接收最新列表元数据 （列表版本、 计数等）。| 
| 201| 已创建| 已创建新的列表。 这被返回初始插入到列表。 该响应包括在列表上保持最新的元数据和位置标头包含列表的 URI。| 
| 304| 未修改| 返回上获取。 指示客户端具有最新版本的列表。 该服务将列表版本<b>If-match</b>标头中的值进行比较。 如果相等，然后 304 获取不返回任何数据。| 
| 400| 错误请求| 请求已格式不正确。 可能的值无效或 URI 或查询字符串参数类型。 也可以是缺少所需的参数的结果或数据值或请求 API 的丢失或无效的版本。 请参阅<b>X XBL 协定版本</b>标头。| 
| 401| 未授权| 请求要求用户身份验证。| 
| 403| 已禁止| 为用户或服务不允许该请求。| 
| 404| 找不到| 调用方没有访问资源。 这指示已创建了任何此类列表。| 
| 412| 前置条件失败| 指示已更改的列表版本和修改请求失败。 修改请求是插入、 更新或删除。 服务会检查列表版本的<b>If-match</b>标头文件。 如果它不匹配当前版本的列表，然后操作将失败，这会返回以及当前列表元数据 （其中包括当前版本）。| 
| 500| 内部服务器错误| 该服务拒绝由于服务器端错误请求。| 
| 501| 未实现| 调用方请求尚未实现在服务器的 URI。 （当前仅时请求使用由列表名称不是列入白名单。）| 
| 503| 服务不可用| 服务器拒绝请求，通常由于过多的负载。 延迟后 (请参阅<b>retry-after</b>标头)，可以重试请求。| 
  
<a id="ID4E5CAC"></a>

 
## <a name="response-body"></a>响应正文
 
<a id="ID4EEDAC"></a>

 
### <a name="sample-response"></a>示例响应
 

```cpp
{ 
"ListMetadata":
  {"ListVersion": 12,
   "ListCount": 6,
   "MaxListSize": 200,
   "AccessSetting": "OwnerOnly",
   "AllowDuplicates": true
  },
"ListItems":
  [{ 
   "Index": 0,
   "DateAdded": "\/Date(1198908717056)/",
   "DateModified": "\/Date(1198908717056)/",
   "HydrationResult": "Indeterminate",
   "HydratedItem": null

   "Item":
   {
     "ContentType": "Movie",
     "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
     "ProviderId": null,
     "Provider": null,
     "ImageUrl": "http://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC",
     "Title": "The Dark Knight",
     "SubTitle": null,
     "Locale": "en-us",
     "AltImageUrl": null,
     "DeviceType": "XboxOne"
    }
  }]
}
         
```

   
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[/ 用户/xuid (xuid) / 列出了/PIN / {listname}](uri-usersxuidlistspinslistname.md)

  
<a id="ID4E1DAC"></a>

 
##### <a name="further-information"></a>详细信息 

[市场 Uri](../marketplace/atoc-reference-marketplace.md)

 [其他参考](../../additional/atoc-xboxlivews-reference-additional.md)

   