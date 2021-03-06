# 一个简单的变形目标

从gl2.0开始定义，支持glTF版本*变形目标*对于网格。变形目标存储特定网格属性的置换或差异。在运行时，可以使用不同的权重将这些差异添加到原始网格中，以便为网格的部分设置动画。这通常用于角色动画，例如，对虚拟角色的不同面部表情进行编码。

下面是一个最小的示例，显示了具有两个变形目标的网格。新的元素将在这里进行总结，变形目标的更广泛的概念以及它们在运行时的应用方式将在下一节中解释。

```json
{
  "scenes":[
    {
      "nodes":[
        0
      ]
    }
  ],
  "nodes":[
    {
      "mesh":0
    }
  ],
  "meshes":[
    {
      "primitives":[
        {
          "attributes":{
            "POSITION":1
          },
          "targets":[
            {
              "POSITION":2
            },
            {
              "POSITION":3
            }
          ],
          "indices":0
        }
      ],
      "weights":[
        1.0,
        0.5
      ]
    }
  ],

  "animations":[
    {
      "samplers":[
        {
          "input":4,
          "interpolation":"LINEAR",
          "output":5
        }
      ],
      "channels":[
        {
          "sampler":0,
          "target":{
            "node":0,
            "path":"weights"
          }
        }
      ]
    }
  ],

  "buffers":[
    {
      "uri":"data:application/gltf-buffer;base64,AAABAAIAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAA/AAAAPwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIC/AACAPwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIA/AACAPwAAAAA=",
      "byteLength":116
    },
    {
      "uri":"data:application/gltf-buffer;base64,AAAAAAAAgD8AAABAAABAQAAAgEAAAAAAAAAAAAAAAAAAAIA/AACAPwAAgD8AAIA/AAAAAAAAAAAAAAAA",
      "byteLength":60
    }
  ],
  "bufferViews":[
    {
      "buffer":0,
      "byteOffset":0,
      "byteLength":6,
      "target":34963
    },
    {
      "buffer":0,
      "byteOffset":8,
      "byteLength":108,
      "byteStride":12,
      "target":34962
    },
    {
      "buffer":1,
      "byteOffset":0,
      "byteLength":20
    },
    {
      "buffer":1,
      "byteOffset":20,
      "byteLength":40
    }
  ],
  "accessors":[
    {
      "bufferView":0,
      "byteOffset":0,
      "componentType":5123,
      "count":3,
      "type":"SCALAR",
      "max":[
        2
      ],
      "min":[
        0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":0,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        1.0,
        0.5,
        0.0
      ],
      "min":[
        0.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":36,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        0.0,
        1.0,
        0.0
      ],
      "min":[
        -1.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":1,
      "byteOffset":72,
      "componentType":5126,
      "count":3,
      "type":"VEC3",
      "max":[
        1.0,
        1.0,
        0.0
      ],
      "min":[
        0.0,
        0.0,
        0.0
      ]
    },
    {
      "bufferView":2,
      "byteOffset":0,
      "componentType":5126,
      "count":5,
      "type":"SCALAR",
      "max":[
        4.0
      ],
      "min":[
        0.0
      ]
    },
    {
      "bufferView":3,
      "byteOffset":0,
      "componentType":5126,
      "count":10,
      "type":"SCALAR",
      "max":[
        1.0
      ],
      "min":[
        0.0
      ]
    }
  ],

  "asset":{
    "version":"2.0"
  }
}


```

资源包含在单个三角形的不同变形目标之间进行插值的动画。此资产的屏幕截图如图17a所示。

![](images/simpleMorph.png)

这个资产的大多数元素已经在前面的部分中解释过了：它包含一个`scene`用一个单曲`节点`还有一张单曲`mesh`. 有两个`缓冲器`对象，一个存储几何数据，另一个存储`animation`，还有几个` 缓冲视图`和`accessor`提供此数据访问权限的对象。

为定义变形目标而添加的新元素包含在`mesh`以及`动画` :

```json
  "meshes":[
    {
      "primitives":[
        {
          "attributes":{
            "POSITION":1
          },
          "targets":[
            {
              "POSITION":2
            },
            {
              "POSITION":3
            }
          ],
          "indices":0
        }
      ],
      "weights":[
        0.5,
        0.5
      ]
    }
  ],


```

这个`mesh.primitive`包含一个数组[变形`targets`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0#morph-targets). 每个变形目标都是将属性名称映射到的字典`存取器`物体。在本例中，有两个变形目标，都映射`"POSITION"`属性设置为包含变形顶点位置的访问器。网格还包含一个数组`重量`定义每个变形目标对最终渲染网格的贡献。这些重量也是`channel.target`的`动画`包含在资产中：

```json
  "animations":[
    {
      "samplers":[
        {
          "input":4,
          "interpolation":"LINEAR",
          "output":5
        }
      ],
      "channels":[
        {
          "sampler":0,
          "target":{
            "node":0,
            "path":"weights"
          }
        }
      ]
    }
  ],


```

这意味着动画将修改`weights`所引用的网格的`目标.node`. 将动画应用于这些权重的结果，以及最终渲染网格的计算，将在下一节中详细介绍[变形目标](morph-targets.md) .