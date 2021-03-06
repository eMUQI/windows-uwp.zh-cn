---
title: 移植着色器对象
description: 移植 OpenGL ES 2.0 中的简单呈现器时，第一步是在 Direct3D 11 中设置等效的顶点着色器和片段着色器对象，并且确保在编译之后主程序能够与着色器对象进行通信。
ms.assetid: 0383b774-bc1b-910e-8eb6-cc969b3dcc08
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 移植, 着色器, direct3d, opengl
ms.localizationpriority: medium
ms.openlocfilehash: b800a32149011376e1d97e0da44d32c733ddfb93
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368228"
---
# <a name="port-the-shader-objects"></a>移植着色器对象




**重要的 Api**

-   [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device)
-   [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)

移植 OpenGL ES 2.0 中的简单呈现器时，第一步是在 Direct3D 11 中设置等效的顶点着色器和片段着色器对象，并且确保在编译之后主程序能够与着色器对象进行通信。

> **请注意**  已创建一个新的 Direct3D 项目？ 如果尚未创建，请按照[为通用 Windows 平台 (UWP) 创建新的 DirectX 11 项目](user-interface.md)中的说明进行操作。 本操作实例假定你已经为绘制到屏幕创建了 DXGI 和 Direct3D 资源（模板中提供了这些资源）。

 

与 OpenGL ES 2.0 非常相似，必须将 Direct3D 中已编译的着色器与绘制上下文相关联。 但是，Direct3D 本身没有着色器程序对象的概念，你必须将着色器直接分配给 [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)。 该步骤遵循 OpenGL ES 2.0 创建和绑定着色器对象的过程，并且为你提供了 Direct3D 中的相应 API 行为。

<a name="instructions"></a>说明
------------

### <a name="step-1-compile-the-shaders"></a>第 1 步：编译着色器

在这个简单的 OpenGL ES 2.0 示例中，着色器存储为文本文件，并作为字符串数据进行加载以供在运行时编译。

OpenGL ES 2.0:编译着色器

``` syntax
GLuint __cdecl CompileShader (GLenum shaderType, const char *shaderSrcStr)
// shaderType can be GL_VERTEX_SHADER or GL_FRAGMENT_SHADER. Returns 0 if compilation fails.
{
  GLuint shaderHandle;
  GLint compiledShaderHandle;
   
  // Create an empty shader object.
  shaderHandle = glCreateShader(shaderType);

  if (shaderHandle == 0)
  return 0;

  // Load the GLSL shader source as a string value. You could obtain it from
  // from reading a text file or hardcoded.
  glShaderSource(shaderHandle, 1, &shaderSrcStr, NULL);
   
  // Compile the shader.
  glCompileShader(shaderHandle);

  // Check the compile status
  glGetShaderiv(shaderHandle, GL_COMPILE_STATUS, &compiledShaderHandle);

  if (!compiledShaderHandle) // error in compilation occurred
  {
    // Handle any errors here.
              
    glDeleteShader(shaderHandle);
    return 0;
  }

  return shaderHandle;

}
```

在 Direct3D 中，着色器不是在运行时期间编译的，当编译程序的其余部分时，它们始终被编译为 CSO 文件。 当使用 Microsoft Visual Studio 编译应用时，HLSL 文件被编译为应用必须加载的 CSO (.cso) 文件。 确保在打包时将这些 CSO 文件与你的应用包含在一起。

> **请注意**  下面的示例执行的着色器加载和使用以异步方式编译**自动**关键字和 lambda 语法。 ReadDataAsync() 是一个为模板实现的方法，它在 CSO 文件中以字节数据数组 (fileData) 的形式进行读取。

 

Direct3D 11:编译着色器

``` syntax
auto loadVSTask = DX::ReadDataAsync(m_projectDir + "SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(m_projectDir + "SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](Platform::Array<byte>^ fileData) {

m_d3dDevice->CreateVertexShader(
  fileData->Data,
  fileData->Length,
  nullptr,
  &m_vertexShader);

auto createPSTask = loadPSTask.then([this](Platform::Array<byte>^ fileData) {
  m_d3dDevice->CreatePixelShader(
    fileData->Data,
    fileData->Length,
    nullptr,
    &m_pixelShader;
};
```

### <a name="step-2-create-and-load-the-vertex-and-fragment-pixel-shaders"></a>步骤 2：创建和加载顶点和片段 （像素） 着色器

OpenGL ES 2.0 包含着色器“程序”的概念，该程序充当在 CPU 上运行的主程序与在 CPU 上执行的着色器之间的接口。 通过一个能够在 GPU 上执行的程序来编译（或从编译的源加载）和关联着色器。

OpenGL ES 2.0:加载到明暗度程序顶点和片段着色器

``` syntax
GLuint __cdecl LoadShaderProgram (const char *vertShaderSrcStr, const char *fragShaderSrcStr)
{
  GLuint programObject, vertexShaderHandle, fragmentShaderHandle;
  GLint linkStatusCode;

  // Load the vertex shader and compile it to an internal executable format.
  vertexShaderHandle = CompileShader(GL_VERTEX_SHADER, vertShaderSrcStr);
  if (vertexShaderHandle == 0)
  {
    glDeleteShader(vertexShaderHandle);
    return 0;
  }

   // Load the fragment/pixel shader and compile it to an internal executable format.
  fragmentShaderHandle = CompileShader(GL_FRAGMENT_SHADER, fragShaderSrcStr);
  if (fragmentShaderHandle == 0)
  {
    glDeleteShader(fragmentShaderHandle);
    return 0;
  }

  // Create the program object proper.
  programObject = glCreateProgram();
   
  if (programObject == 0)    return 0;

  // Attach the compiled shaders
  glAttachShader(programObject, vertexShaderHandle);
  glAttachShader(programObject, fragmentShaderHandle);

  // Compile the shaders into binary executables in memory and link them to the program object..
  glLinkProgram(programObject);

  // Check the project object link status and determine if the program is available.
  glGetProgramiv(programObject, GL_LINK_STATUS, &linkStatusCode);

  if (!linkStatusCode) // if link status <> 0
  {
    // Linking failed; delete the program object and return a failure code (0).

    glDeleteProgram (programObject);
    return 0;
  }

  // Deallocate the unused shader resources. The actual executables are part of the program object.
  glDeleteShader(vertexShaderHandle);
  glDeleteShader(fragmentShaderHandle);

  return programObject;
}

// ...

glUseProgram(renderer->programObject);
```

Direct3D 没有着色器程序对象的概念。 当在 [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 接口上调用其中一个着色器创建方法（如 [**ID3D11Device::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) 或 [**ID3D11Device::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)）时会创建着色器。 为了能够为当前绘制上下文设置着色器，我们为它们提供了相应的 [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)，它包含一个设置的着色器方法，如用于顶点着色器的 [**ID3D11DeviceContext::VSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) 或用于片段着色器的 [**ID3D11DeviceContext::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)。

Direct3D 11:设置图形设备绘图上下文的着色器。

``` syntax
m_d3dContext->VSSetShader(
  m_vertexShader.Get(),
  nullptr,
  0);

m_d3dContext->PSSetShader(
  m_pixelShader.Get(),
  nullptr,
  0);
```

### <a name="step-3-define-the-data-to-supply-to-the-shaders"></a>步骤 3:定义要提供给着色器的数据

在我们的 OpenGL ES 2.0 示例中，我们有一个要为着色器管道声明的 **uniform**：

-   **u\_mvpMatrix**： 浮点数表示采用该模型的最终模型-视图-投影转换矩阵的 4x4 数组协调对多维数据集并将转换的扫描转换二维投影坐标。

顶点数据的两个 **attribute** 值：

-   **\_位置**: 4 浮点矢量中供模型的顶点坐标。
-   **\_颜色**:4 浮点向量中用来与顶点关联的 RGBA 颜色值。

打开 GL ES 2.0:GLSL 制服和属性的定义

``` syntax
uniform mat4 u_mvpMatrix;
attribute vec4 a_position;
attribute vec4 a_color;
```

在本例中，相应的主程序变量定义为呈现器对象上的字段。 (请参阅中的标头[如何： 移植到 Direct3D 11 的简单 OpenGL ES 2.0 呈现器](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)。)我们已经完成的我们需要在内存中，其中的主要程序将提供的着色器管道中，我们通常执行此操作之前的绘图调用的这些值指定的位置：

OpenGL ES 2.0:标记一致性和属性数据的位置

``` syntax

// Inform the shader of the attribute locations
loc = glGetAttribLocation(renderer->programObject, "a_position");
glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), 0);
glEnableVertexAttribArray(loc);

loc = glGetAttribLocation(renderer->programObject, "a_color");
glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
    sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);


// Inform the shader program of the uniform location
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");
```

Direct3D 没有同样含义的“attribute”或“uniform”的概念（或者至少它不会共享该语法）。 它具有常量缓冲区，该缓冲区以 Direct3D 子资源形式表示，即：在主程序和着色器程序之间共享的资源。 其中一些子资源（如顶点位置和颜色）以 HLSL 语义的形式进行描述。 有关与 OpenGL ES 2.0 概念相关时的常量缓冲区和 HLSL 语义的详细信息，请阅读[移植帧缓冲区对象、uniform 和 attribute](porting-uniforms-and-attributes.md)。

将此过程移动到 Direct3D 时，我们将 uniform 转换为 Direct3D 常量缓冲区 (cbuffer) 并使用 **register** HLSL 语义为其分配一个用于查找的寄存器。 两个顶点属性作为着色器管道阶段的输入元素进行处理，而且还分配了通知着色器的 [HLSL 语义](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps)（POSITION 和 COLOR0）。 像素着色器采用 SV\_位置，与 SV\_前缀指示它由 GPU 生成一个系统值。 （在这种情况下，它是在扫描转换过程中生成的像素位置。）按常量放入缓冲区，这是因为前者将被用来定义顶点缓冲区不声明 VertexShaderInput 和 PixelShaderInput (请参阅[顶点缓冲区和数据端口](port-the-vertex-buffers-and-data-config.md))，并且后者的数据生成的结果为在管道中，在这种情况下是顶点着色器的上一阶段。

Direct3D:HLSL 常量缓冲区和顶点数据定义

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float4 pos : POSITION;
  float4 color : COLOR0;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

有关移植到常量缓冲区以及应用 HLSL 语义的详细信息，请阅读[移植帧缓冲区对象、uniform 和 attribute](porting-uniforms-and-attributes.md)。

下面是通过常量缓冲区或顶点缓冲区传递到着色器管道的数据布局的结构。

Direct3D 11:声明常量和顶点缓冲区布局

``` syntax
// Constant buffer used to send MVP matrices to the vertex shader.
struct ModelViewProjectionConstantBuffer
{
  DirectX::XMFLOAT4X4 modelViewProjection;
};

// Used to send per-vertex data to the vertex shader.
struct VertexPositionColor
{
  DirectX::XMFLOAT4 pos;
  DirectX::XMFLOAT4 color;
};
```

使用 DirectXMath XM\*类型在常量缓冲区元素，因为它们提供适当的打包和内容的对齐方式发送到着色器管道时。 如果使用标准的 Windows 平台浮点类型和数组，那么你必须自行执行打包和对齐操作。

若要绑定的常量缓冲区，创建作为布局描述[ **CD3D11\_缓冲区\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-cd3d11_buffer_desc)结构，并将其传递给[ **ID3DDevice::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)。 然后，在你的呈现方法中，将常量缓冲区传递给 [**ID3D11DeviceContext::UpdateSubresource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource)，之后再进行绘制。

Direct3D 11:将常量缓冲区绑定

``` syntax
CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);

m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);

// ...

// Only update shader resources that have changed since the last frame.
m_d3dContext->UpdateSubresource(
  m_constantBuffer.Get(),
  0,
  NULL,
  &m_constantBufferData,
  0,
  0);
```

创建和更新顶点缓冲区非常类似，下一步[移植顶点缓冲区和数据](port-the-vertex-buffers-and-data-config.md)中对此进行了介绍。

<a name="next-step"></a>下一步
---------

[顶点缓冲区和数据端口](port-the-vertex-buffers-and-data-config.md)
## <a name="related-topics"></a>相关主题


[如何： 移植到 Direct3D 11 的简单 OpenGL ES 2.0 呈现器](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[顶点缓冲区和数据端口](port-the-vertex-buffers-and-data-config.md)

[GLSL 端口](port-the-glsl.md)

[在屏幕上绘制](draw-to-the-screen.md)

 

 




