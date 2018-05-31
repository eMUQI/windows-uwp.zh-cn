---
author: stevewhims
description: 本主题介绍了可用于在 C++/CX 和 C++/WinRT 对象之间转换的两个帮助程序函数。
title: 实现 C++/WinRT 与 C++/CX 之间的互操作
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, 互操作, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 616bd9ea8c4b89599e703ef9467206b028fd596b
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1831781"
---
# <a name="interop-between-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-and-ccx"></a>实现 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 与 C++/CX 之间的互操作
本主题介绍了可用于在 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 和 C++/WinRT 对象之间转换的两个帮助程序函数。 你可以借助这些技术，为使用两个语言投影的代码实现互操作，也可以在将代码从 C++/CX 逐步迁移到 C++/WinRT 时使用这些技术。

## <a name="fromcx-and-tocx-functions"></a>from_cx 和 to_cx 函数
下面的帮助程序函数将 C++/CX 对象转换为等效的 C++/WinRT 对象。 该函数将 C++/CX 对象强制转换为其基础 [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) 接口指针。 然后，它对该指针调用 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) 来查询 C++/WinRT 对象的默认接口。 **QueryInterface** 是 C++/CX safe_cast 扩展的 Windows 运行时应用程序二进制接口 (ABI) 等效项。 此外，[**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) 函数将检索 C++/WinRT 对象的基础 **IUnknown** 接口指针的地址，使该地址能够设置为其他值。

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };
 
    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
                         reinterpret_cast<void**>(winrt::put_abi(to))));
 
    return to;
}
```

下面的帮助程序函数将 C++/WinRT 对象转换为等效的 C++/CX 对象。 [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) 函数将检索指向 C++/WinRT 对象的基础 **IUnknown** 接口的指针。 在使用 C++/CX safe_cast 扩展查询请求的 C++/CX 类型之前，此函数会将该指针强制转换为 C++/CX 对象。

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="code-example"></a>代码示例
下面是介绍正在使用的两个帮助程序函数的代码示例（基于 C++/CX **空白应用**项目模板）。 它还演示可以处理信息的方式命名空间冲突跨两个投影。

```cppwinrt
// MainPage.xaml.cpp

#include "pch.h"
#include "MainPage.xaml.h"
#include <winrt/Windows.Foundation.h>
#include <sstream>

using namespace InteropExample;

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows::Foundation;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

MainPage::MainPage()
{
    InitializeComponent();

    winrt::init_apartment(winrt::apartment_type::single_threaded);

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);
}
```

## <a name="important-apis"></a>重要的 API
* [IUnknown 接口](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [winrt::get_abi 函数](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 函数](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)