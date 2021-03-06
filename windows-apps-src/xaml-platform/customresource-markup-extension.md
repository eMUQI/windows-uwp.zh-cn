---
description: 通过计算对来源于自定义资源查找实现的资源的引用，为任何 XAML 属性提供值。 资源查找是通过 CustomXamlResourceLoader 类实现执行的。
title: CustomResource 标记扩展
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d59a8662d430c527aa2f83aea945ee5bcf48642e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366428"
---
# <a name="customresource-markup-extension"></a>{CustomResource} 标记扩展


通过计算对来源于自定义资源查找实现的资源的引用，为任何 XAML 属性提供值。 资源查找是通过 [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 类实现执行的。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 描述 |
|------|-------------|
| 键 | 所请求资源的键。 键的初始分配方式特定于当前注册使用的 [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 类的实现。 |

## <a name="remarks"></a>备注

**CustomResource** 这种技术可获取在自定义资源存储库中的其他地方定义的值。 此技术相对比较高级，大多数 Windows 运行时应用方案都没有使用此技术。

本主题中不介绍 **CustomResource** 解析资源词典的方法，因为解析方法会随 [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 实现方法的不同而有很大的不同。

只要在标记中遇到使用 `{CustomResource}` 的情况，就由 Windows 运行时 XAML 分析程序调用 [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 实现的 [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 方法。 传递给 **GetResource** 的 *resourceId* 来自 *key* 参数，其他输入参数来自上下文（例如使用情况所应用的属性）。

默认情况下，`{CustomResource}` 使用情况不起作用。（[**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 的基本实现不完整）。 要进行有效的 `{CustomResource}` 引用，必须执行以下每一个步骤：

1.  从 [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 派生自定义类，并替代 [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 方法。 不要在实现中调用基类。
2.  设置 [**CustomXamlResourceLoader.Current**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) 以在初始化逻辑中引用你的类。 必须在加载包括 `{CustomResource}` 扩展在内的任何页面级 XAML 之前执行此操作。 设置 **CustomXamlResourceLoader.Current** 的一个位置是，App.xaml 代码隐藏模板中为你生成的 [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) 子类构造函数。
3.  现在你可以在你的应用加载为页面的 XAML 中，或从 XAML 资源词典内使用 `{CustomResource}` 扩展。

**CustomResource** 是标记扩展。 当需要将属性值转义为除文字值或处理程序名称之外的值时，以及当需求更具全局性而不是仅仅将类型转换器放在某些类型或属性上时，通常需要实现标记扩展。 在 XAML 使用的所有标记扩展"\{"和"\}"约定所依据的 XAML 处理器识别标记扩展必须处理该属性其特性语法中的字符。

## <a name="related-topics"></a>相关主题

* [ResourceDictionary 和 XAML 资源引用](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
* [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)

