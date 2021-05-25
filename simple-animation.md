# 简单的动画

如图所示[场景和节点](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_004_ScenesNodes.md)节，每个节点都可以有一个局部变换。此转换可以由`matrix`属性或使用`翻译` ,`rotation`，和`规模` （TRS）属性

当转换由TRS属性给出时，一个[`animation`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-animation)可以用来描述`翻译` ,`rotation`，或`规模`节点的值会随时间而变化。

以下是[最小glTF文件](a-minimal-gltf-file.md)这在前面显示过，但通过动画进行了扩展。本节将解释为添加此动画所做的更改和扩展。

```json
{
  "scenes" : [
    {
      "nodes" : [ 0 ]
    }
  ],
  
  "nodes" : [
    {
      "mesh" : 0,
      "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
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
  
  "animations": [
    {
      "samplers" : [
        {
          "input" : 2,
          "interpolation" : "LINEAR",
          "output" : 3
        }
      ],
      "channels" : [ {
        "sampler" : 0,
        "target" : {
          "node" : 0,
          "path" : "rotation"
        }
      } ]
    }
  ],

  "buffers" : [
    {
      "uri" : "data:application/octet-stream;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAA=",
      "byteLength" : 44
    },
    {
      "uri" : "data:application/octet-stream;base64,AAAAAAAAgD4AAAA/AABAPwAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAD0/TQ/9P00PwAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAPT9ND/0/TS/AAAAAAAAAAAAAAAAAACAPw==",
      "byteLength" : 100
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
    },
    {
      "buffer" : 1,
      "byteOffset" : 0,
      "byteLength" : 100
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
      "bufferView" : 2,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 5,
      "type" : "SCALAR",
      "max" : [ 1.0 ],
      "min" : [ 0.0 ]
    },
    {
      "bufferView" : 2,
      "byteOffset" : 20,
      "componentType" : 5126,
      "count" : 5,
      "type" : "VEC4",
      "max" : [ 0.0, 0.0, 1.0, 1.0 ],
      "min" : [ 0.0, 0.0, 0.0, -0.707 ]
    }
  ],
  
  "asset" : {
    "version" : "2.0"
  }
  
}
```

![](images/animatedTriangle.gif)

## 这个`rotation`的属性`节点`

示例中唯一的节点现在具有`rotation`财产。这是一个数组，包含[四元数](https://en.wikipedia.org/wiki/Quaternion)它描述了旋转：

```json
"nodes" : [
    {
      "mesh" : 0,
      "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
    }
  ],
```

给定的值是描述“0度旋转”的四元数，因此三角形将以其初始方向显示。

## 动画数据

glTF JSON的顶层数组中添加了三个元素来编码动画数据：

- 一个新的`buffer`包含原始动画数据；
- 一个新的`bufferView`指缓冲区；
- 两个新的`accessor`向动画数据添加结构信息的对象。

### 这个`buffer`以及` 缓冲视图`对于原始动画数据

一个新的`buffer`已添加，其中包含原始动画数据。此缓冲区还使用[数据URI](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_002_BasicGltfStructure.md#binary-data-in-data-uris)要对动画数据包含的100个字节进行编码，请执行以下操作：

```json
"buffers" : [
    ...
    {
      "uri" : "data:application/octet-stream;base64,AAAAAAAAgD4AAAA/AABAPwAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAD0/TQ/9P00PwAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAPT9ND/0/TS/AAAAAAAAAAAAAAAAAACAPw==",
      "byteLength" : 100
    }
  ],

  "bufferViews" : [
    ...
    {
      "buffer" : 1,
      "byteOffset" : 0,
      "byteLength" : 100
    }
  ],
```

还有一个新的`bufferView`，这里只指`缓冲器`包含整个动画缓冲区数据的索引1。进一步的结构信息添加了`accessor`下面描述的对象

请注意，还可以将动画数据附加到已包含三角形几何体数据的现有缓冲区。在这种情况下，新的缓冲区视图将引用`buffer`索引为0，并使用适当的` 字节偏移量`引用缓冲区中包含动画数据的部分。

在这里显示的示例中，动画数据被添加为新的缓冲区，以保持几何体数据和动画数据分离。

### 这个`accessor`动画数据的对象

两个新的`accessor`添加了描述如何解释动画数据的对象。第一个访问器描述*时代*动画关键帧的。有五个元素（如`count`每一个都是标量`浮动`值（总共20个字节）。第二个访问器说，在前20个字节之后，有五个元素，每个元素都是一个4D向量，其中`float`组件。这些是*旋转*对应于动画的五个关键帧，以四元数表示。

```json
"accessors" : [
    ...
    {
      "bufferView" : 2,
      "byteOffset" : 0,
      "componentType" : 5126,
      "count" : 5,
      "type" : "SCALAR",
      "max" : [ 1.0 ],
      "min" : [ 0.0 ]
    },
    {
      "bufferView" : 2,
      "byteOffset" : 20,
      "componentType" : 5126,
      "count" : 5,
      "type" : "VEC4",
      "max" : [ 0.0, 0.0, 1.0, 1.0 ],
      "min" : [ 0.0, 0.0, 0.0, -0.707 ]
    }
  ],
```

由提供的实际数据*时代*存取器和*旋转*访问器使用示例中缓冲区的数据，如下表所示：

| *时代*存取器          | *旋转*存取器              | 意义                                                         |
| --------------------- | ------------------------- | ------------------------------------------------------------ |
| zero                  | (0.0, 0.0, 0.0, 1.0 )     | 0.0秒时，三角形旋转0度                                       |
| zero point two five   | (0.0, 0.0, 0.707, 0.707)  | 在0.25秒时，它绕z轴旋转90度                                  |
| zero point five       | (0.0, 0.0, 1.0, 0.0)      | 在0.5秒时，它绕z轴旋转180度                                  |
| zero point seven five | (0.0, 0.0, 0.707, -0.707) | At 0.75 seconds, it has a rotation of 270 (= -90) degrees around the z-axis |
| one                   | (0.0, 0.0, 0.0, 1.0)      | At 1.0 seconds, it has a rotation of 360 (= 0) degrees around the z-axis |

所以这个动画描述了围绕z轴旋转360度，持续1秒。

## 这个`animation`

最后，这是添加实际动画的部分。最高层`animations`数组包含一个`动画`对象。它包括两个要素：

- 这个`samplers`，描述动画数据的来源；
- 这个`channels`，可以想象为将动画数据的“源”连接到“目标”

在给定的示例中，有一个采样器。每个采样器定义`input`还有一个`输出`财产。它们都引用访问器对象。这里，这些是*时代*访问器（带索引2）和*旋转*上述访问器（索引为3）。此外，采样器定义`interpolation`类型，即`“线性”`在这个例子中

还有一个`channel`在示例中。此通道引用唯一的采样器（索引为0）作为动画数据的源。动画的目标编码在`频道.目标`对象：它包含`id`引用其属性应设置动画的节点。实际的节点属性在`路径`. 所以给定示例中的通道目标表示`"rotation"`应为索引为0的节点的属性设置动画。

```json
"animations": [
    {
      "samplers" : [
        {
          "input" : 2,
          "interpolation" : "LINEAR",
          "output" : 3
        }
      ],
      "channels" : [ {
        "sampler" : 0,
        "target" : {
          "node" : 0,
          "path" : "rotation"
        }
      } ]
    }
  ],
```

结合所有这些信息，给定的动画对象将显示以下内容：

> 在动画期间，动画值从*旋转*存取器。它们是基于当前模拟时间和由提供的关键帧时间进行线性插值的*时代*存取器。然后将插值值写入`"rotation"`索引为0的节点的属性。

有关插值和计算的更详细描述和实际示例，请参见[动画](animations.md)第节