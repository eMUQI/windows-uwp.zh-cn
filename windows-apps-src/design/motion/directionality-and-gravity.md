---
Description: 了解如何 Fluent motion 使用方向性和重力。
title: 方向性和引力 - UWP 应用中的动画
label: Directionality and gravity
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8f1e36f0febeeaac5a12d408d7be8a717f0ab398
ms.sourcegitcommit: 7c3b88198178d6f6a535f35e1bf8665410d41d92
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569116"
---
# <a name="directionality-and-gravity"></a>方向性和引力

方向信号有助于巩固用户在体验过程中建立的心理模型。 任何运动的方向都必须支持空间连续性和空间中对象的完整性。

定向运动受到力（例如引力）的影响。 对运动施加力可让运动看起来更加自然。

## <a name="examples"></a>示例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果已安装了 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，单击此处<a href="xamlcontrolsgallery:/category/Motion">打开此应用，查看“动作”的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>运动方向

:::row:::
    :::column:::
移动的方向对应于物理运动。 就像在自然界中一样，对象可以沿任意世界轴（X、Y、Z）移动。 我们按照这种方式看待对象在屏幕上的运动。
当移动对象时，避免非自然冲突。 请的注意对象来自并转到，并始终在场景中，如滚动方向或布局层次结构支持可能使用的更高级构造。
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>导航方向

应用中场景之间的导航方向是概念性的。 用户前进和后退导航。 场景移入和移出视图。 这些概念结合物理运动为用户提供指引。

当导航导致对象从前一个场景移动到新场景时，该对象在屏幕上进行简单的 A 到 B 移动。 为了让运动效果更加逼真，增加了标准缓动和引力感。

对于后退导航，移动是反向的（从 B 到 A）。 当用户导航回来时，他们希望尽快返回到之前的状态。 计时更快、更直接，并使用减速缓动。

下面的示例应用了上述原则 - 在前进和后退导航过程中，所选项目停留在屏幕上。

![UI 连续动作示例](images/continuous3.gif)

当导航导致屏幕上的项目被替换时，显示退出场景去往何方、新场景来自何处至关重要。

这有以下几个好处：

- 巩固用户的空间心理模型。
- 退出场景的持续时间提供更多时间来准备内容，以便为进入场景应用动画效果。
- 改善了应用的感知性能。

需要仔细考虑 4 个导航方向。

:::row:::
    :::column:::
**正向中**庆祝内容不包含传出内容冲突的方式输入场景。 内容在场景减速。
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**进扩展**内容很快退出。 关闭屏幕加速对象。
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**向后接**与正向中相同但反转。
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**向后扩展**进扩展相同但反转。
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>引力

引力可让体验更加自然。 沿 Z 轴移动并且没有通过屏幕提供 (affordance) 锚定到场景的对象可能会受到引力的影响。 当对象摆脱场景约束但没有达到逃逸速度之前，引力会向下拉扯对象，使对象在移动时产生更自然的曲线轨迹。

引力效果通常在对象必须从一个场景跳到另一个场景时显现。 因此，连接的动画使用了引力的概念。

在下面的示例中，网格顶行中的一个元素受到引力的影响，致使其在离开原来位置时略微下降，然后才移动到前方。

![方向后退进入](images/continuity-photos.gif)

## <a name="related-articles"></a>相关文章

- [Motion 概述](index.md)
- [计时和缓动](timing-and-easing.md)
