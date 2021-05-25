# glTF_tutorial
本教程介绍了gl传输格式glTF。它总结了glTF的最重要的功能和应用案例，并描述了与glTF相关的文件的结构。它说明了如何读取，处理和使用glTF资产来有效地显示3D图形。

# 使用WebGL介绍glTF

越来越多的应用程序和服务是基于3D内容的。在线商店提供3D预览的产品配置程序。博物馆通过三维扫描将其文物数字化，并允许游客在虚拟画廊中探索他们的藏品。城市规划者使用三维城市模型进行规划和信息可视化。教育工作者创造交互式的，动画的人体三维模型。其中许多应用程序直接在web浏览器中运行，这是可能的，因为所有现代浏览器都支持使用WebGL进行高效渲染。

各种应用对3D内容的需求不断增加。在许多情况下，3D内容必须通过web传输，并且必须在客户端高效地呈现。但是到目前为止，3D内容的创建和运行时应用程序中3D内容的高效呈现之间还存在差距。

## 3D内容管道

在客户端应用程序中呈现的三维内容来自不同的源，并以不同的文件格式存储。这个[维基百科上的三维图形文件格式列表](https://en.wikipedia.org/wiki/List_of_file_formats#3D_graphics)显示了一个压倒性的数字，有超过70种不同的三维数据文件格式，服务于不同的目的和应用案例。

例如，可以使用3D扫描仪获得原始3D数据。这些扫描仪通常提供单个对象的几何数据，这些数据存储在[对象](https://en.wikipedia.org/wiki/Wavefront_.obj_file) ,[使用](https://en.wikipedia.org/wiki/PLY_(file_format))，或[STL公司](https://en.wikipedia.org/wiki/STL_(file_format))文件夹。这些文件格式不包含有关场景结构或对象应如何渲染的信息。

可以使用创作工具创建更复杂的三维场景。这些工具允许用户编辑场景的结构、灯光设置、摄影机、动画，当然还有场景中显示的对象的三维几何体。应用程序以自己的自定义文件格式存储这些信息。例如，[搅拌机](https://www.blender.org/)将场景存储在`.blend`文件夹，[光波3D](https://www.lightwave3d.com/)使用`.lws`文件格式，[3ds Max](http://www.autodesk.com/3dsmax)使用`.max`文件格式，以及[玛雅](http://www.autodesk.com/maya)使用`.ma`文件夹

为了呈现这样的3D内容，运行时应用程序必须能够读取不同的输入文件格式。场景结构必须被解析，3D几何数据必须转换成图形API所需的格式。三维数据必须传输到显卡存储器，然后用图形API调用序列来描述渲染过程。因此，每个运行时应用程序都必须为它将支持的所有文件格式创建导入程序、加载程序或转换器，

![contentPipeline](images/contentPipeline.png)

## glTF：一种三维场景传输格式

glTF的目标是定义一个表示3D内容的标准，其形式适合在运行时应用程序中使用。现有的文件格式不适合这个用例：有些文件不包含任何场景信息，而只包含几何体数据；另一些则是为在创作应用程序之间交换数据而设计的，它们的主要目标是尽可能多地保留有关3D场景的信息，从而生成通常大、复杂、难以解析的文件。此外，可能必须对几何数据进行预处理，以便可以使用客户端应用程序对其进行渲染。

现有的文件格式都不是为在web上高效传输3D场景并尽可能高效地渲染它们而设计的。但glTF并不是“另一种文件格式”*传输*3D场景格式：

- 场景结构用JSON描述，结构紧凑，易于解析。
- 对象的3D数据以可由公共图形api直接使用的形式存储，因此不存在解码或预处理3D数据的开销。

不同的内容创建工具现在可以提供glTF格式的3D内容。越来越多的客户机应用程序能够使用和呈现glTF。因此，glTF可能有助于弥合内容创建和呈现之间的差距，

![contentPipelineWithGltf](images/contentPipelineWithGltf.png)

越来越多的内容创建工具将能够直接提供glTF。或者，可以使用其他文件格式创建glTF资产，方法是使用[Khronos glTF储存库](https://github.com/KhronosGroup/glTF#converters). 例如，几乎所有创作应用程序都可以在[科拉达](https://www.khronos.org/collada/)格式。所以[ COLLADA2GLTF公司](https://github.com/KhronosGroup/COLLADA2GLTF)该工具可用于将场景和模型从这些创作应用程序转换为glTF。`OBJ`可以使用glTF文件转换为[ obj2gltf公司](https://github.com/AnalyticalGraphicsInc/obj2gltf). 对于其他文件格式，可以使用自定义转换器创建glTF资产，从而使3D内容可用于广泛的运行时应用程序。

