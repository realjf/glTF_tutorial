# 网格

这个[简单网格](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_008_SimpleMeshes.md)上一节中的示例显示了[`mesh`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-mesh)用一个[`mesh.primitive`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-primitive)包含多个属性的。本节将解释网格基本体的含义和用法，如何将网格附加到场景图的节点，以及如何使用不同的材质渲染它们。

## 网格图元

每个`mesh`包含一个数组`网格.基本体`物体。这些网格基本体对象是较大对象的较小部分或构建块。网格图元汇总了有关如何渲染对象的各个部分的所有信息。

### 网格基本体属性

网格图元使用`attributes`字典。此几何数据通过参考`存取器`包含顶点属性数据的对象。关于`accessor`概念在[缓冲区、缓冲区视图和访问器](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_005_BuffersBufferViewsAccessors.md)第节

在给定的示例中，在`attributes`字典。这些条目是指` 位置接受者`以及`normalsAccessor` :

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

这些访问器的元素一起定义属于各个顶点的属性，如图9a所示。

![](images/meshPrimitiveAttributes.png)

### 索引和非索引几何图形

a的几何数据`mesh.primitive`可能是*索引*几何体或没有索引的几何体。在给定的示例中`mesh.primitive`包含*索引*几何学。这由`indices`属性，它引用索引为0的访问器，定义索引的数据。对于非索引几何体，将忽略此属性。

### 网格基本体模式

默认情况下，假设几何体数据描述三角形网格。对于*索引*几何，这意味着`indices`存取器假设包含单个三角形的索引。对于非索引几何体，假设顶点属性访问器的三个元素包含三角形的三个顶点的属性。

也可以使用其他渲染模式：几何体数据也可以描述单独的点、线或三角形条带。这由`mode`可以存储在网格图元中。表示如何解释其数据的常量值。例如，模式可以是` zero`当几何图形由点组成时，或`4`当它由三角形组成时。这些常数对应于GL常数`点数`或`TRIANGLES`分别是。见[`primitive.mode`规范](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#primitivemode)获取可用模式的列表

### 网格基本体材质

网格图元也可以引用`material`应使用此材质的索引进行渲染。在给定的例子中，没有`材料`，使对象使用仅定义对象的50%均匀灰色的默认材质进行渲染。材料和相关概念的详细说明将在[材料](materials.md)第节

## 附加到节点的网格

在示例中[简单网格](simple-mesh.md)部分，有一个`scene`，它包含两个节点，并且两个节点引用相同的节点`网格`实例，其索引为0：

```
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
    { ... } 
  ],
```

第二个节点有一个`translation`财产。如图所示[场景和节点](scene-and-nodes.md)节，这将用于计算此节点的局部变换矩阵。在这种情况下，矩阵将导致沿x轴平移1.0。节点的所有局部变换的乘积将生成[全局变换](scene-and-nodes.md#global-transforms-of-nodes). 所有附加到节点上的元素都将使用这个全局变换进行渲染。

因此，在本例中，网格将被渲染两次，因为它附加到两个节点：一次是第一个节点的全局变换，即标识变换；一次是第二个节点的全局变换，即沿x轴平移1.0。