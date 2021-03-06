---
description: 本指南介绍如何直接在 WPF 和 Windows 窗体应用程序中创建基于 Fluent 的 UWP UI
title: 桌面应用中的 UWP 控件
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 96705faff278c4cab31e0ab271bc31d08261401b
ms.sourcegitcommit: 1455e12a50f98823bfa3730c1d90337b1983b711
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76814007"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>在桌面应用中托管 UWP XAML 控件（XAML 岛）

从 Windows 10 版本 1903 开始，可以使用称为“XAML 岛”  的功能在非 UWP 桌面应用程序中托管 UWP 控件。 可以通过此功能来增强现有 WPF、Windows 窗体和 C++ Win32 应用程序的外观和功能，并使用只能通过 UWP 控件使用的最新 Windows 10 UI 功能。 这意味着，可以在现有的 WPF、Windows 窗体和 C++ Win32 应用程序中使用 UWP 功能（例如 [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions)）和支持 [Fluent Design System](/windows/uwp/design/fluent-design-system/index) 的控件。

可以托管派生自 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的任何 UWP 控件，其中包括：

* Windows SDK 或 WinUI 库提供的任何第一方 UWP 控件。
* 任何自定义 UWP 控件（例如，包含多个可一起使用的 UWP 控件的用户控件）。 必须有自定义控件的源代码，才能通过应用程序对其进行编译。

从根本上讲，XAML 岛使用 UWP XAML 托管 API  创建。 此 API 包含 Windows 10 版本 1903 SDK 中引入的几个 Windows 运行时类和 COM 接口。 我们还在 [Windows 社区工具包](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)中提供了一组 XAML 岛 .NET 控件（此工具包在内部使用 UWP XAML 托管 API），并针对 WPF 和 Windows 窗体应用提供了更方便的开发体验。

使用 XAML 岛的方式取决于应用程序类型和要托管的 UWP 控件类型。

> [!NOTE]
> 如果有关于 XAML 岛的反馈，请在 [Microsoft.Toolkit.Win32 存储库](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues)中创建一个新问题，将你的意见留在那里。 若要私下提交反馈，可将其发送到 XamlIslandsFeedback@microsoft.com。 你的意见和方案对我们至关重要。

## <a name="wpf-and-windows-forms-applications"></a>WPF 和 Windows 窗体应用程序

我们的建议是让 WPF 和 Windows 窗体应用程序使用 Windows 社区工具包中提供的 XAML 岛 .NET 控件。 这些控件提供一个对象模型，用于模拟相应 UWP 控件的属性、方法和事件（或提供对它们的访问权限）。 它们还处理键盘导航和布局更改等行为。

有两组用于 WPF 和 Windows 窗体应用程序的 XAML 岛控件：包装控件  和主机控件  。 

### <a name="wrapped-controls"></a>包装控件

WPF 和 Windows 窗体应用程序可以使用选定的 XAML 岛控件来包装特定 UWP 控件的界面和功能。 可以在 WPF 或 Windows 窗体项目的设计图面中直接添加这些控件，然后在设计器中像使用任何其他 WPF 或 Windows 窗体控件那样使用它们。

下述 UWP 包装控件目前在 Windows 社区工具包中提供。 

| 控件 | 支持的 OS 的最低版本 | 说明 |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10 版本 1903 | 为 Windows 窗体或 WPF 桌面应用程序中基于 Windows Ink 的用户交互提供图面和相关的工具栏。 |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10 版本 1903 | 嵌入一个用于在 Windows 窗体或 WPF 桌面应用程序中流式传输和呈现媒体内容（如视频）的视图。 |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10 版本 1903 | 使你能够在 Windows 窗体或 WPF 桌面应用程序中显示符号地图或相片级地图。 |

若要详细演示如何使用 UWP 包装控件，请参阅[在 WPF 应用中托管标准 UWP 控件](host-standard-control-with-xaml-islands.md)。

### <a name="host-controls"></a>主机控件

如果方案超出可用包装控件的涵盖范围，WPF 和 Windows 窗体应用程序也可使用 Windows 社区工具包中提供的 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控件。

| 控件 | 支持的 OS 的最低版本 | 说明 |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10 版本 1903 | 可以托管派生自 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的任何 UWP 控件，包括 Windows SDK 提供的任何第一方 UWP 控件以及自定义控件。 |

若要详细演示如何使用 **WindowsXamlHost** 控件，请参阅[在 WPF 应用中托管标准 UWP 控件](host-standard-control-with-xaml-islands.md)和[使用 XAML 岛在 WPF 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands.md)。

> [!NOTE]
> 只能在以 .NET Core 3 为目标的 WPF 和 Windows 窗体应用中使用 **WindowsXamlHost** 控制来托管自定义 UWP 控件。 在以 .NET Framework 或 .NET Core 3 为目标的应用中，可以托管 Windows SDK 提供的第一方 UWP 控件。

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>将项目配置为使用 XAML 岛 .NET 控件

XAML 岛 .NET 控件需要 Windows 10 版本 1903 或更高版本。 若要使用这些控件，请安装下面列出的 NuGet 包之一。 这些包提供了使用 XAML 岛包装控件和主机控件所需的一切，并包括其他也是必需的相关 NuGet 包。

| 控件类型 | NuGet 包  | 相关文章 |
|-----------------|----------------|---------------------|
| [包装控件](#wrapped-controls) | 以下包的 6.0.0 版或更高版本： <ul><li>WPF：[Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows 窗体：[Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [在 WPF 应用中托管标准 UWP 控件](host-standard-control-with-xaml-islands.md)  |
| [主机控件](#host-controls) | 以下包的 6.0.0 版或更高版本： <ul><li>WPF：[Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows 窗体：[Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [在 WPF 应用中托管标准 UWP 控件](host-standard-control-with-xaml-islands.md)<br/>[在 WPF 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands.md)  |

请注意以下详细信息：

* 主机控件包也包括在包装控件包中。 若要使用这两组控件，可以安装包装控件包。

* 若要托管自定义 UWP 控件，则 WPF 或 Windows 窗体项目必须以 .NET Core 3 为目标。 以 .NET Framework 为目标的应用不支持托管自定义 UWP 控件。 还需执行一些附加步骤才能引用自定义控件。 有关详细信息，请参阅[使用 XAML 岛在 WPF 应用中托管自定义 UWP 控件](host-custom-control-with-xaml-islands.md)。

* 这些说明的早期版本已要求你将 `maxversiontested` 元素添加到 WPF 或 Windows 窗体项目中的应用程序清单。 只要你使用上面列出的最新版 NuGet 包，就不再需要将此元素添加到清单中。

### <a name="architecture-of-xaml-island-net-controls"></a>XAML 岛 .NET 控件的体系结构

下面从体系结构方面简要介绍了如何将不同类型的 XAML 岛控件组织到 UWP XAML 托管 API 之上。

![主机控件体系结构](images/xaml-islands/host-controls.png)

此图表底部所示的 API 随 Windows SDK 提供。 包装控件和主机控件可通过 Windows 社区工具包中的 NuGet 包获得。

### <a name="web-view-controls"></a>Web 视图控件

Windows 社区工具包还提供了以下 .NET 控件，用于在 WPF 和 Windows 窗体应用程序中托管 Web 内容的。 这些控件通常以 XAML 岛控件形式用于类似的桌面应用现代化方案，并保留在 XAML 岛控件所在的 [Microsoft.Toolkit.Win32 存储库](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32)中。

| 控件 | 支持的 OS 的最低版本 | 说明 |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10 版本 1803 | 使用 Microsoft Edge 呈现引擎来显示 Web 内容。 |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | 提供与更多 OS 版本兼容的 **WebView** 版本。 此控件使用 Microsoft Edge 呈现引擎在 Windows 10 版本 1803 及更高版本上显示 Web 内容，使用 Internet Explorer 呈现引擎在早期版本的 Windows 10、Windows 8.x 和 Windows 7 上显示 Web 内容。 |

若要使用这些控件，请安装以下 NuGet 包之一：

* WPF：[Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows 窗体：[Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++ Win32 应用程序

C++ Win32 应用程序不支持 XAML 岛 .NET 控件。 这些应用程序必须改用 Windows 10 SDK（版本 1903 及更高版本）提供的 UWP XAML 托管 API  。

UWP XAML 托管 API 包含多个 Windows 运行时类和 COM 接口，可供 C++ Win32 应用程序用来托管派生自 [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 的任何 UWP 控件。 可以在具有关联窗口句柄 (HWND) 的应用程序的任何 UI 元素中托管 UWP 控件。 有关此 API 的详细信息（包括先决条件），请参阅[在 C++ Win32 应用中使用 UWP XAML 托管 API](using-the-xaml-hosting-api.md)。

> [!NOTE]
> Windows 社区工具包中的包装控件和主机控件在内部使用 UWP XAML 托管 API，并实现你在直接使用 UWP XAML 托管 API 时需要自行处理的所有行为，包括键盘导航和布局更改。 对于 WPF 和 Windows 窗体应用程序，强烈建议使用这些控件而不是直接使用 UWP XAML 托管 API，因为它们抽象掉了使用 API 的许多实现细节。

## <a name="feature-roadmap"></a>功能路线图

下面是从 Windows 10 版本 1903 和 Windows 社区工具包发布版 6.0 开始推出的 XAML 岛相关功能的当前状态：

* **C++ Win32 应用：** 从 Windows 10 版本 1903 开始推出的 UWP XAML 托管 API 被视为 1.0 版。
* **以 .NET Framework 4.6.2 及更高版本为目标的托管应用：** 对于以 .NET Framework 4.6.2 及更高版本为目标的应用，在 [6.0.0 版 NuGet 包](#configure-your-project-to-use-the-xaml-island-net-controls)中提供的 XAML 岛控件被视为 1.0 版。
* **以 .NET Core 3.0 及更高版本为目标的托管应用：** 对于以 .NET Core 3.0 及更高版本为目标的应用，在 [6.0.0 版 NuGet 包](#configure-your-project-to-use-the-xaml-island-net-controls)中提供的控件仍为开发者预览版。 用于 .NET Core 3.0 及更高版本的这些控件的 1.0 发布版计划用于更高版本。

## <a name="additional-resources"></a>其他资源

有关如何使用 XAML 岛的更多背景信息和教程，请参阅以下文章和资源：

* [WPF 应用现代化教程](modernize-wpf-tutorial.md)：本教程提供的分步说明介绍了如何使用 Windows 社区工具包中的包装控件和主机控件向现有的 WPF 业务线应用程序添加 UWP 控件。 本教程包含 WPF 应用程序的完整代码，以及该过程中每个步骤的详细说明。
* [XAML 岛代码示例](https://github.com/microsoft/Xaml-Islands-Samples)：此存储库包含的 Windows 窗体、WPF 和 C++/Win32 示例演示了如何使用 XAML 岛。
* [XAML Islands v1 - Updates and Roadmap](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap)（XAML 岛 v1 - 更新和路线图）：此博客文章讨论了有关 XAML 岛的许多常见问题，并提供了详细的开发路线图。
