---
title: "线列表"
description: "线列表是隔离的直线段的列表。 线列表用于向 3D 场景添加雨夹雪或暴雨之类的任务。 应用程序通过填充一组顶点来创建线列表。"
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords: "线列表"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 8ebe6c67bd6b68023f59599cdcbd1f8f44f80e84
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="line-lists"></a>线列表


线列表是隔离的直线段的列表。 线列表用于向 3D 场景添加雨夹雪或暴雨之类的任务。 应用程序通过填充一组顶点来创建线列表。 请注意，线列表中的顶点数必须是大于或等于 2 的偶数。

-   [示例](#example)
-   [相关主题](#related-topics)

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>示例


下图显示了渲染的线列表。

![线列表示意图](images/linelst.png)

你可以向线列表应用材质和纹理。 材质或纹理中的颜色仅沿绘制的线显示，而不是线之间的任何点。

下面的代码显示如何为此线列表创建顶点。

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

下面的代码示例显示如何在 Direct3D 中渲染线列表。

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINELIST, 0, 3 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[基元](primitives.md)

 

 



