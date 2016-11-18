---
author: mcleanbyron
description: "使用 Windows 应用商店提交 API 中的此方法终止软件包外部测试版的软件包推出。"
title: "使用 Windows 应用商店提交 API 终止软件包外部测试版的软件包推出"
translationtype: Human Translation
ms.sourcegitcommit: 9b76a11adfab838b21713cb384cdf31eada3286e
ms.openlocfilehash: d51c2fa2babb78a35317ab846685530f5c3b4a16

---

# 使用 Windows 应用商店提交 API 终止软件包外部测试版的软件包推出


使用 Windows 应用商店提交 API 中的此方法[终止软件包外部测试版提交的软件包推出](../publish/gradual-package-rollout.md#completing-the-rollout)。 有关通过使用 Windows 应用商店提交 API 创建软件包外部测试版提交过程的详细信息，请参阅[管理软件包外部测试版提交](manage-flight-submissions.md)。

## 先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 使用你的开发人员中心帐户为应用创建提交。 可以使用开发人员中心仪表板执行此操作，也可以使用[创建应用提交](create-an-app-submission.md)方法执行此操作。
* 启用提交的逐步软件包推出。 可以使用[开发人员中心仪表板](../publish/gradual-package-rollout.md)执行此操作，也可以[使用 Windows 应用商店提交 API](manage-flight-submissions.md#manage-gradual-package-rollout) 执行此操作。

>**注意**&nbsp;&nbsp;此方法只可以用于授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。

## 请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求参数的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout``` |

<span/>
 

### 请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |

<span/>

### 请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字符串 | 必需。 应用（包含要终止软件包推出的软件包外部测试版提交）的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| flightId | 字符串 | 必需。 软件包外部测试版（包含要终止软件包推出的提交）的 ID。 可通过开发人员中心仪表板获取此 ID，它包含在[创建软件包外部测试版](create-a-flight.md)和[获取应用的软件包外部测试版](get-flights-for-an-app.md)请求的响应数据中。  |
| submissionId | 字符串 | 必需。 要终止软件包推出的提交的 ID。 可通过开发人员中心仪表板获取此 ID，它包含在[创建软件包外部测试版提交](create-a-flight-submission.md)请求的响应数据中。  |

<span/>

### 请求正文

请勿为此方法提供请求正文。

### 请求示例

以下示例演示了如何终止软件包外部测试版提交的软件包推出。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## 响应

以下示例演示了成功调用此方法的 JSON 响应正文。 有关响应正文中这些值的更多详细信息，请参阅[软件包推出资源](manage-flight-submissions.md#package-rollout-object)。

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 0,
    "packageRolloutStatus": "PackageRolloutStopped",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## 错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 404  | 找不到软件包外部测试版提交。 |
| 409  | 此代码指示以下错误之一：<br/><br/><ul><li>对于逐步推出操作，该提交未处于有效状态（在调用此方法之前，必须发布该提交，并且必须将 [packageRolloutStatus](manage-flight-submissions.md#package-rollout-object) 值设置为 **PackageRolloutInProgress**）。</li><li>提交不属于指定应用。</li><li>应用使用的开发人员中心仪表板功能[当前不受 Windows 应用商店提交 API 支持](create-and-manage-submissions-using-windows-store-services.md#not_supported)。</li></ul> |   

<span/>


## 相关主题

* [逐步软件包推出](../publish/gradual-package-rollout.md)
* [使用 Windows 应用商店提交 API 管理软件包外部测试版提交](manage-flight-submissions.md)
* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->

