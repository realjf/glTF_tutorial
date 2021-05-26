# 简单的皮肤

glTF支架*顶点蒙皮*，允许网格的几何体（顶点）根据骨架的姿势变形。这对于为动画几何体（例如虚拟角色）提供逼真的外观非常重要。在glTF资源中定义顶点蒙皮的核心是[`skin`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-skin)，但顶点蒙皮通常意味着glTF资源的元素之间的几个相互依赖关系，这些元素到目前为止已经介绍过。

下面是一个glTF资源，它显示了简单几何体的基本顶点蒙皮。此资源的元素将在本节中快速总结，适当时参考前面的部分，并指出为顶点蒙皮功能添加的新元素。下一节将给出顶点蒙皮的详细信息和背景信息。

```json
{
  "scenes" : [ {
    "nodes" : [ 0 ]
  } ],
  
  "nodes" : [ {
    "skin" : 0,
    "mesh" : 0,
    "children" : [ 1 ]
  }, {
    "children" : [ 2 ],
    "translation" : [ 0.0, 1.0, 0.0 ]
  }, {
    "rotation" : [ 0.0, 0.0, 0.0, 1.0 ]
  } ],
  
  "meshes" : [ {
    "primitives" : [ {
      "attributes" : {
        "POSITION" : 1,
        "JOINTS_0" : 2,
        "WEIGHTS_0" : 3
      },
      "indices" : 0
    } ]
  } ],

  "skins" : [ {
    "inverseBindMatrices" : 4,
    "joints" : [ 1, 2 ]
  } ],
  
  "animations" : [ {
    "channels" : [ {
      "sampler" : 0,
      "target" : {
        "node" : 2,
        "path" : "rotation"
      }
    } ],
    "samplers" : [ {
      "input" : 5,
      "interpolation" : "LINEAR",
      "output" : 6
    } ]
  } ],
  
  "buffers" : [ {
    "uri" : "data:application/gltf-buffer;base64,AAABAAMAAAADAAIAAgADAAUAAgAFAAQABAAFAAcABAAHAAYABgAHAAkABgAJAAgAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAAD8AAAAAAACAPwAAAD8AAAAAAAAAAAAAgD8AAAAAAACAPwAAgD8AAAAAAAAAAAAAwD8AAAAAAACAPwAAwD8AAAAAAAAAAAAAAEAAAAAAAACAPwAAAEAAAAAA",
    "byteLength" : 168
  }, {
    "uri" : "data:application/gltf-buffer;base64,AAABAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAABAPwAAgD4AAAAAAAAAAAAAQD8AAIA+AAAAAAAAAAAAAAA/AAAAPwAAAAAAAAAAAAAAPwAAAD8AAAAAAAAAAAAAgD4AAEA/AAAAAAAAAAAAAIA+AABAPwAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAA=",
    "byteLength" : 320
  }, {
    "uri" : "data:application/gltf-buffer;base64,AACAPwAAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAAAAAAAAgD8AAAAAAAAAvwAAgL8AAAAAAACAPwAAgD8AAAAAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAAAAAAAAAAAAAAIA/AAAAAAAAAL8AAIC/AAAAAAAAgD8=",
    "byteLength" : 128
  }, {
    "uri" : "data:application/gltf-buffer;base64,AAAAAAAAAD8AAIA/AADAPwAAAEAAACBAAABAQAAAYEAAAIBAAACQQAAAoEAAALBAAAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAkxjEPkSLbD8AAAAAAAAAAPT9ND/0/TQ/AAAAAAAAAAD0/TQ/9P00PwAAAAAAAAAAkxjEPkSLbD8AAAAAAAAAAAAAAAAAAIA/AAAAAAAAAAAAAAAAAACAPwAAAAAAAAAAkxjEvkSLbD8AAAAAAAAAAPT9NL/0/TQ/AAAAAAAAAAD0/TS/9P00PwAAAAAAAAAAkxjEvkSLbD8AAAAAAAAAAAAAAAAAAIA/",
    "byteLength" : 240
  } ],
  
  "bufferViews" : [ {
    "buffer" : 0,
    "byteOffset" : 0,
    "byteLength" : 48,
    "target" : 34963
  }, {
    "buffer" : 0,
    "byteOffset" : 48,
    "byteLength" : 120,
    "target" : 34962
  }, {
    "buffer" : 1,
    "byteOffset" : 0,
    "byteLength" : 320,
    "byteStride" : 16
  }, {
    "buffer" : 2,
    "byteOffset" : 0,
    "byteLength" : 128
  }, {
    "buffer" : 3,
    "byteOffset" : 0,
    "byteLength" : 240
  } ],

  "accessors" : [ {
    "bufferView" : 0,
    "byteOffset" : 0,
    "componentType" : 5123,
    "count" : 24,
    "type" : "SCALAR",
    "max" : [ 9 ],
    "min" : [ 0 ]
  }, {
    "bufferView" : 1,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 10,
    "type" : "VEC3",
    "max" : [ 1.0, 2.0, 0.0 ],
    "min" : [ 0.0, 0.0, 0.0 ]
  }, {
    "bufferView" : 2,
    "byteOffset" : 0,
    "componentType" : 5123,
    "count" : 10,
    "type" : "VEC4",
    "max" : [ 0, 1, 0, 0 ],
    "min" : [ 0, 1, 0, 0 ]
  }, {
    "bufferView" : 2,
    "byteOffset" : 160,
    "componentType" : 5126,
    "count" : 10,
    "type" : "VEC4",
    "max" : [ 1.0, 1.0, 0.0, 0.0 ],
    "min" : [ 0.0, 0.0, 0.0, 0.0 ]
  }, {
    "bufferView" : 3,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 2,
    "type" : "MAT4",
    "max" : [ 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, -0.5, -1.0, 0.0, 1.0 ],
    "min" : [ 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, -0.5, -1.0, 0.0, 1.0 ]
  }, {
    "bufferView" : 4,
    "byteOffset" : 0,
    "componentType" : 5126,
    "count" : 12,
    "type" : "SCALAR",
    "max" : [ 5.5 ],
    "min" : [ 0.0 ]
  }, {
    "bufferView" : 4,
    "byteOffset" : 48,
    "componentType" : 5126,
    "count" : 12,
    "type" : "VEC4",
    "max" : [ 0.0, 0.0, 0.707, 1.0 ],
    "min" : [ 0.0, 0.0, -0.707, 0.707 ]
  } ],
  
  "asset" : {
    "version" : "2.0"
  }
}

```

渲染该资源的结果如图19a所示。

![](images/simpleSkin.gif)

## 简单皮肤示例的元素

下面简要总结了给定示例的要素：

- 这个`scenes`和`节点`元素已在[场景和节点](scene-and-nodes.md)第节。对于顶点蒙皮，添加了新节点：索引1和索引2处的节点为*骨架*. 这些节点可以被认为是最终导致网格变形的“骨骼”之间的关节。
- 新的顶级词典`skins`在给定示例中包含单个皮肤。稍后将解释此皮肤对象的属性。
- 概念`animations`已经在[动画](animations.md)第节。在给定的示例中，动画引用*骨架*使顶点蒙皮的效果在动画期间实际上可见。
- 这个[网格](meshes.md)章节已经解释了`meshes`和`网格.基本体`物体。在本例中，添加了顶点蒙皮所需的新网格基本体属性，即`"JOINTS_0"`和`“权重\u 0”`属性
- 有几个新的`buffers` ,` 缓冲视图`，和`accessors`. 它们的基本特性在[缓冲区、缓冲区视图和访问器](buffer-bufferviews-accessors.md)第节。在给定的示例中，它们包含顶点蒙皮所需的附加数据。

有关这些元素如何互连以实现顶点蒙皮的详细信息将在[皮肤](skins.md)第节