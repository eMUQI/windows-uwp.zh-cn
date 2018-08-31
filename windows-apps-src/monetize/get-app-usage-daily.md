---
author: Xansky
ms.assetid: 99DB5622-3700-4FB2-803B-DA447A1FD7B7
description: 在 Microsoft Store 分析 API 中使用此方法，可获取给定的日期范围和其他可选筛选器的每日应用使用情况数据。
title: 获取每日应用使用情况
ms.author: mhopkins
ms.date: 08/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，应用商店服务，Microsoft Store 分析 API，使用情况
ms.localizationpriority: medium
ms.openlocfilehash: 5060c24df7242d62e2895231d7441e904987d522
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2018
ms.locfileid: "3244348"
---
# <a name="get-daily-app-usage"></a>获取每日应用使用情况

在 Microsoft Store 分析 API 中使用此方法采用 JSON 格式的聚合的使用情况数据 （不包括 Xbox 多人游戏） 获取给定的日期范围 （过去 90 天内仅） 和其他可选筛选器内某一应用程序。 此信息也是在 Windows 开发人员中心仪表板中的[使用情况报告](../publish/usage-report.md)中可用。

## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                               |
|--------|---------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数     | 类型   |  说明                                                                                                    |  必需  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 字符串 | 要检索评价数据的应用的 [Store ID](in-app-purchases-and-trials.md#store-ids)。 |  是       |
| startDate     | date   | 要检索的评价数据日期范围中的开始日期。 默认值为当前日期。                   |  否        |
| endDate       | date   | 要检索的评价数据日期范围中的结束日期。 默认值为当前日期。                     |  否        |
| top           | int    | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。                          |  否        |
| skip          | int    | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。                         |  否        |  
| filter        |字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 eq 或 ne 运算符进行关联，并且语句可以使用 and 或 or 进行组合。 filter 参数中的字符串值必须使用单引号括起来。 你可以指定响应正文中的以下字段： <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | 否         |  
| orderby       | 字符串 | 对结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p><em>order</em> 参数是可选的，可以是 **asc** 或 **desc**，用于指定每个字段的升序或降序排列。 默认值为 **asc**。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p>                                                                                                   |  否        |
| groupby       | 字符串 | 仅将数据聚合应用于指定字段的语句。 你可以指定响应正文中的以下字段： <ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p>                                                                                                             |  否        |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求用于获取每日应用使用情况数据。 将 *applicationId* 值替换为你的应用的 Store ID。

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily?applicationId=XXXXXXXXXXXX&startDate=2018-08-10&endDate=2018-08-14 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应



### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| 值      | array  | 包含聚合的使用情况数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下表格。 |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的评价数据超过 10000 行时，就会返回此值。                 |
| TotalCount | int    | 查询的数据结果中的行总数。                                                                          |

 
### <a name="usage-values"></a>使用情况值

*Value* 数组中的元素包含以下值。

| 值                     | 类型    | 说明                                                               |
|---------------------------|---------|---------------------------------------------------------------------------|
| date                      | 字符串  | 有关使用情况数据日期范围中的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。        |
| applicationId             | 字符串  | 检索使用情况数据的应用商店 ID。          |
| applicationName           | 字符串  | 应用的显示名称。                                              |
| deviceType                | 字符串  | 以下字符串之一，指定使用情况发生的位置的设备的类型：<ul><li>**电脑**</li><li>**电话**</li><li>**控制台**</li><li>**Tablet**</li><li>**IoT**</li><li>**服务器**</li><li>**全息**</li><li>**未知**</li></ul>                                                                                                         |
| packageVersion            | 字符串  | 使用情况发生的位置的程序包的版本。                          |
| market                    | 字符串  | 客户使用你的应用的市场的 ISO 3166 国家/地区代码。 |
| subscriptionName          | 字符串  | 指示使用情况是通过 Xbox Game Pass。                            |
| dailySessionCount         | 长型    | 在这一天的用户会话数量。                                  |
| engagementDurationMinutes | Double  | 用户在其中主动使用你的应用的不同的时间段，在应用启动时启动测量 （进程开始） 或终止 （进程结束） 或非活动状态一段时间后结束分钟。             |
| dailyActiveUsers          | 长型    | 使用这一天的应用的客户数量。                           |
| dailyActiveDevices        | 长型    | 每日用于与应用交互的所有用户的设备的数量。  |
| dailyNewUsers             | 长型    | 第一次那一天使用你的应用的客户数量。    |
| monthlyActiveUsers        | 长型    | 使用该月的应用的客户数量。                         |
| monthlyActiveDevices      | 长型    | 许多不同的一段时间，在应用启动时启动 （进程开始） 运行你的应用和结束时终止 （进程结束） 的设备或处于非活动状态一段时间后。                                      |
| monthlyNewUsers           | 长型    | 使用你的应用第一次该月的客户的数量。  |


### <a name="response-example"></a>回复示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2018-08-10",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 17718,
      "engagementDurationMinutes": 582540.2,
      "dailyActiveUsers": 7078,
      "dailyActiveDevices": 6964,
      "dailyNewUsers": 1727,
      "monthlyActiveUsers": 123609,
      "monthlyActiveDevices": 116723,
      "monthlyNewUsers": 72271
    },
    {
      "date": "2018-08-11",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 19899,
      "engagementDurationMinutes": 655379.7,
      "dailyActiveUsers": 7877,
      "dailyActiveDevices": 7789,
      "dailyNewUsers": 2062,
      "monthlyActiveUsers": 123666,
      "monthlyActiveDevices": 116770,
      "monthlyNewUsers": 72276
    },
    {
      "date": "2018-08-12",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 21349,
      "engagementDurationMinutes": 735640.5,
      "dailyActiveUsers": 8456,
      "dailyActiveDevices": 8309,
      "dailyNewUsers": 2433,
      "monthlyActiveUsers": 124241,
      "monthlyActiveDevices": 117289,
      "monthlyNewUsers": 72732
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取每月应用 ussage](get-app-usage-monthly.md)
* [获取应用购置](get-app-acquisitions.md)
* [获取加载项购置](get-in-app-acquisitions.md)
* [获取错误报告数据](get-error-reporting-data.md)