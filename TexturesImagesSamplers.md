# 纹理、图像和采样器

纹理是赋予对象逼真外观的一个重要方面。它们可以定义对象的主颜色以及材质定义中使用的其他特性，以便精确地描述渲染对象的外观。

glTF资产可以定义多个[`texture`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-texture)对象，可在渲染期间用作几何对象的纹理，并可用于编码不同的材质属性。根据图形API的不同，可能有许多特性和设置会影响纹理映射的过程。许多这些细节都超出了本教程的范围。有专门的教程，解释所有纹理映射参数和设置的确切含义；例如，在[ webglfundamentals.org网站](http://webglfundamentals.org/webgl/lessons/webgl-3d-textures.html) ,[打开.gl](https://open.gl/textures)，以及其他。本节将仅概述有关纹理的信息是如何在glTF资源中编码的。

gltfjson中有三个顶级数组用于定义纹理。这个`textures` ,`采样器`，和`images`字典包含[`texture`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-texture) ,[`sampler`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-sampler)，和[`image`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-image)对象。以下节选自[质感简单](SimpleTexture.md)示例，将在下一节中介绍：

```json
"textures": [
  {
    "source": 0,
    "sampler": 0
  }
],
"images": [
  {
    "uri": "testTexture.png"
  }
],
"samplers": [
  {
     "magFilter": 9729,
     "minFilter": 9987,
     "wrapS": 33648,
     "wrapT": 33648
   }
],
```

这个`texture`它本身使用索引来引用一个`采样器`还有一个`image`. 这里最重要的元素是引用`形象`. 它包含一个URI，链接到将用于纹理的实际图像文件。有关如何读取此图像数据的信息，请参见[图像数据输入`images`](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_002_BasicGltfStructure.md#image-data-in-images) .

下一节将展示如何在材质内部使用这种纹理定义。