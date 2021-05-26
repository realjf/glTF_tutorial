# 简单网格

A[`mesh`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-mesh)表示出现在场景中的几何对象。一个示例`网格`已经显示在[最小glTF文件](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_003_MinimalGltfFile.md). 这个例子有一个`mesh`连接到一个单一的`节点`，网格由一个[`mesh.primitive`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-primitive)它只包含一个属性，即顶点位置的属性。但是通常，网格基本体将包含更多属性。例如，这些属性可以是顶点法线或纹理坐标。

以下是一个包含多个属性的简单网格的glTF资源，它将作为解释相关概念的基础：

```
{
  "scenes" : [
    {
      "nodes" : [ 0, 1]
    }
  ],
  "nodes" : [
    {
      "mesh" : 0
    },
    {
      "mesh" : 0,
      "translation" : [ 1.0, 0.0, 0.0 ]
    }
  ],
  
  "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1,
          "NORMAL" : 2
        },
        "indices" : 0
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAgD8AAAAAAAAAAAAAgD8=",
      "byteLength" : 80
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
      "byteLength" : 72,
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
    },
    {
      "bufferView" : 1,
      "byteOffset" : 36,
      "componentType" : 5126,
      "count" : 3,
      "type" : "VEC3",
      "max" : [ 0.0, 0.0, 1.0 ],
      "min" : [ 0.0, 0.0, 1.0 ]
    }
  ],
  
  "asset" : {
    "version" : "2.0"
  }
}
```

图8a显示了渲染的glTF资源。

![](images/simpleMeshes.png)

## 网格定义

给定的示例仍然包含具有单个网格基本体的单个网格。但此网格基本体包含多个属性：

```
 "meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1,
          "NORMAL" : 2
        },
        "indices" : 0
      } ]
    }
  ],
```

除了`"POSITION"`属性，它有一个`“正常”`属性。这是指`accessor`对象提供顶点法线，如中所述[缓冲区、缓冲区视图和访问器]()第节

## 渲染网格实例

如图8a所示，网格被渲染*两次*. 这是通过将网格附加到两个不同的节点来完成的：

```
"nodes" : [
    {
      "mesh" : 0
    },
    {
      "mesh" : 0,
      "translation" : [ 1.0, 0.0, 0.0 ]
    }
  ],
```

这个`mesh`属性引用使用网格索引附加到节点的网格。其中一个节点具有`翻译`这将导致附加网格在不同的位置进行渲染。

这个[下一节](meshes.md)将更详细地解释网格和网格基本体。