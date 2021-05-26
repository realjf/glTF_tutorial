# 简单的材料

前面几节中给出的glTF资源示例包含基本场景结构和简单的几何对象。但是它们没有包含关于物体外观的信息。如果没有提供此类信息，则鼓励查看者使用“默认”材质渲染对象。如屏幕截图所示[最小glTF文件](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_003_MinimalGltfFile.md)，根据场景中的灯光条件，此默认材质会使对象以统一的白色或浅灰色渲染。

本节将从一个非常简单的材料示例开始，并解释不同材料特性的影响。

这是一个最小的glTF资产，具有简单的材料：

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
        "indices" : 0,
        "material" : 0
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

  "materials" : [
    {
      "pbrMetallicRoughness": {
        "baseColorFactor": [ 1.000, 0.766, 0.336, 1.0 ],
        "metallicFactor": 0.5,
        "roughnessFactor": 0.1
      }
    }
  ],
  "asset" : {
    "version" : "2.0"
  }
}
```

渲染时，该资源将使用新材质显示三角形，如图11a所示。

![](images/simpleMaterial.png)

## 材料定义

在gltfjson中添加了一个新的顶级数组来定义这个材质：`materials`数组包含定义材质及其属性的单个元素：

```json
 "materials" : [
    {
      "pbrMetallicRoughness": {
        "baseColorFactor": [ 1.000, 0.766, 0.336, 1.0 ],
        "metallicFactor": 0.5,
        "roughnessFactor": 0.1
      }
    }
  ],
```

的实际定义[`material`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-material)这里只包括[`pbrMetallicRoughness`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-pbrmetallicroughness)对象中定义材质的基本属性*金属粗糙度模型*. (因此，所有其他材料属性将具有默认值，稍后将对此进行解释。）`baseColorFactor`包含材质主颜色的红色、绿色、蓝色和alpha分量，这里是明亮的橙色。这个` 金属因子`0.5表示材料的反射特性应介于金属和非金属材料之间。这个`roughnessFactor`使材质不是完美的镜面状，而是将反射光散射一点。

## 将材质指定给对象

将材质指定给三角形，即`mesh.primitive`，通过使用其索引参考材料：

```json
"meshes" : [
    {
      "primitives" : [ {
        "attributes" : {
          "POSITION" : 1
        },
        "indices" : 0,
        "material" : 0
      } ]
    }
```

下一节将简要介绍如何在glTF资源中定义纹理。使用纹理可以定义更复杂、更真实的材质。