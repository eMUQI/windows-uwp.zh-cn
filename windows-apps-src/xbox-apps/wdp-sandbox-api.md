---
title: Device Portal Xbox Live 沙盒 API 参考
description: 了解如何以编程方式访问 Xbox Live 沙盒。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 72c7459c-420a-4da9-8afa-191a846185a5
ms.localizationpriority: medium
ms.openlocfilehash: 8f04514962cf0684daa99ee75d4c4da73c785735
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244083"
---
# <a name="xbox-live-sandbox-api-reference"></a>Xbox Live 沙盒 API 参考   
你可以使用此 REST API 获取和设置 Xbox Live 沙盒。

## <a name="get-the-xbox-live-sandbox"></a>获取 Xbox Live 沙盒

**请求**

你可以利用以下请求来读取设备的 Xbox Live 沙盒的当前值：

方法      | 请求 URI
:------     | :-----
GET | /ext/xboxlive/sandbox

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
Sandbox -（字符串）设备当前所在的沙盒。   

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 描述
:------     | :-----
200 | 已授予访问文件共享凭据的请求。
4XX | 错误代码
5XX | 错误代码

## <a name="set-the-xbox-live-sandbox"></a>设置 Xbox Live 沙盒
你可以利用以下请求来更改设备的 Xbox Live 沙盒。 请注意，在 Xbox One 上，设备需要重新启动，设置才会生效。

**请求**

你可以利用以下请求来设置设备的 Xbox Live 沙盒的当前值：

方法      | 请求 URI
:------     | :-----
PUT | /ext/xboxlive/sandbox

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**   
请求正文是一个 JSON 对象，包含以下字段：   
Sandbox -（字符串）要将设备的沙盒设置为该值的新值。

**响应**   
Sandbox -（字符串）设备当前所在的沙盒。   

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 描述
:------     | :-----
200 | 已授予访问文件共享凭据的请求。
4XX | 错误代码
5XX | 错误代码

**可用设备系列**

* Windows Xbox

