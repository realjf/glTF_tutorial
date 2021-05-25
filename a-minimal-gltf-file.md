# 最小的glTF文件

下面是一个最小但完整的glTF资产，包含一个单独的索引三角形。您可以复制并粘贴到`gltf`文件，每个基于glTF的应用程序都应该能够加载和呈现它。本节将基于这个例子解释glTF的基本概念。

```json
{
  "scenes" : [
    {
      "nodes" : [ 0 ]
    }
  ],
  
  "nodes" : [
    {
      "mesh" : 0
    }
  ],
  
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    }
  ],
  "bufferViews" : [
    {
      "buffer" : 0,
      "byteOffset" : 0,
      "byteLength" : 6,
      "target" : 34963
    },
    {
      "buffer" : 0,
      "byteOffset" : 8,
      "byteLength" : 36,
      "target" : 34962
    }
  ],
  "accessors" : [
    {
      "bufferView" : 0,
      "byteOffset" : 0,
      "componentType" : 5123,
      "count" : 3,
      "type" : "SCALAR",
      "max" : [ 2 ],
      "min" : [ 0 ]
    },
    {
      "bufferView" : 1,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 3,
      "type" : "VEC3",
      "max" : [ 1.0, 1.0, 0.0 ],
      "min" : [ 0.0, 0.0, 0.0 ]
    }
  ],
  
  "asset" : {
    "version" : "2.0"
  }
}
```

![triangle](images/triangle.png)

## 这个`scene`和`节点`结构

这个[`scene`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-scene)是存储在glTF中的场景描述的入口点。解析glTF JSON文件时，场景结构的遍历将从这里开始。每个场景都包含一个名为`节点`，其中包含[`node`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-node)物体。这些节点是场景图层次的根节点。

这里的示例由一个场景组成。它指的是本例中唯一的节点，即索引为0的节点。而该节点又指索引为0的唯一网格：

```json
 "scenes" : [
    {
      "nodes" : [ 0 ]
    }
  ],
  
  "nodes" : [
    {
      "mesh" : 0
    }
  ],
```

有关场景和节点及其属性的详细信息将在[场景和节点](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_004_ScenesNodes.md)第节

## 这个`meshes`

A[`mesh`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-mesh)表示出现在场景中的实际几何对象。网格本身通常没有任何属性，但只包含[`mesh.primitive`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-primitive)对象，作为较大模型的构建块。每个网格图元都包含网格所包含的几何体数据的描述。

该示例由一个网格组成，并且有一个`mesh.primitive`对象。网格基本体有一个数组`属性`. 这些是网格几何体顶点的属性，在本例中，这只是`POSITION`属性，描述顶点的位置。基本体描述网格*索引*几何图形，由`indices`财产。默认情况下，假设它描述一组三角形，因此三个连续的索引是一个三角形顶点的索引。

网格图元的实际几何数据由`attributes`以及`指数`. 它们都是指`accessor`对象，将在下面解释

```json
"meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0
      } ]
    }
  ],
```

有关网格和网格基本体的详细描述，请参见[网格](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_009_Meshes.md)第节

## 这个`buffer` ,` 缓冲视图`，和`accessor`概念

这个`buffer` ,` 缓冲视图`，和`accessor`对象提供有关网格基本体所包含的几何体数据的信息。基于具体的例子，我们很快就介绍了它们。有关这些概念的更详细的描述将在[缓冲区、缓冲区视图和访问器](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_005_BuffersBufferViewsAccessors.md)第节

### 缓冲器

A[`buffer`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-buffer)非结构化数据定义了一个没有固有意义的原始数据块。它包含一个`uri`，它可以指向包含数据的外部文件，也可以是[数据URI](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_002_BasicGltfStructure.md#binary-data-in-data-uris)它直接在JSON文件中对二进制数据进行编码。

在示例文件中，使用第二种方法：有一个包含44个字节的缓冲区，该缓冲区的数据被编码为数据URI：

```json
"buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    }
  ],
```

此数据包含三角形的索引和三角形的顶点位置。但是为了实际使用这些数据作为网格图元的几何数据，有关*结构*此数据是必需的。有关结构的信息编码在`bufferView`和`存取器`物体

### 缓冲区视图

A[`bufferView`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-bufferView)描述整个原始缓冲区数据的“块”或“片”。在给定的示例中，有两个缓冲区视图。它们都指向同一个缓冲区。第一个缓冲区视图是指包含索引数据的缓冲区部分：它有一个` 字节偏移量`表示整个缓冲区数据的0，以及`byteLength`第二个缓冲区视图是指包含顶点位置的缓冲区部分。从a开始` 字节偏移量`有一个`byteLength`36个；也就是说，它延伸到整个缓冲区的末尾。

```json
"bufferViews" : [
    {
      "buffer" : 0,
      "byteOffset" : 0,
      "byteLength" : 6,
      "target" : 34963
    },
    {
      "buffer" : 0,
      "byteOffset" : 8,
      "byteLength" : 36,
      "target" : 34962
    }
  ],
```

### 访问器

结构数据的第二步是用[`accessor`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-accessor)物体。它们定义了` 缓冲视图`必须通过提供有关数据类型和布局的信息来解释。

在本例中，有两个访问器对象。

第一个访问器描述几何数据的索引。它指的是`bufferView`索引为0，它是`缓冲器`包含索引的原始数据。此外，它还指定`count`和`类型`元素和它们的`componentType`. 在本例中，有3个标量元素，它们的组件类型由一个表示`无符号短`类型

第二个访问器描述顶点位置。它包含对缓冲区数据相关部分的引用，通过`bufferView`索引为1，并且`计数` ,`type`，和` 组件类型`属性表示三维向量有三个元素，每个元素都有`float`组件

```json
"accessors" : [
    {
      "bufferView" : 0,
      "byteOffset" : 0,
      "componentType" : 5123,
      "count" : 3,
      "type" : "SCALAR",
      "max" : [ 2 ],
      "min" : [ 0 ]
    },
    {
      "bufferView" : 1,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 3,
      "type" : "VEC3",
      "max" : [ 1.0, 1.0, 0.0 ],
      "min" : [ 0.0, 0.0, 0.0 ]
    }
  ],
```

如上所述`mesh.primitive`现在可以引用这些访问器，使用它们的索引：

```json
"meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0
      } ]
    }
  ],
```

什么时候`mesh.primitive`渲染器可以解析底层的缓冲区视图和缓冲区，并将缓冲区的所需部分连同有关数据类型和布局的信息一起发送给渲染器。有关呈现器如何获取和处理访问器数据的更详细描述，请参见[缓冲区、缓冲区视图和访问器](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_005_BuffersBufferViewsAccessors.md)节和[材料与工艺](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_013_MaterialsTechniques.md)第节

## 这个`asset`描述

在gltf1.0中，这个属性仍然是可选的，但是在随后的glTF版本中，JSON文件必须包含`asset`包含`版本`数字。这里的示例说明资产符合glTF版本2.0：

```json
 "asset" : {
    "version" : "2.0"
  }
```

这个`asset`属性可能包含中描述的其他元数据[`asset`规范](https://github.com/KhronosGroup/glTF/blob/master/specification/README.md#reference-asset) .