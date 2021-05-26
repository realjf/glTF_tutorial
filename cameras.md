# 摄像机

中的示例[简单的照相机](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_017_SimpleCameras.md)本节介绍了如何定义透视和正交摄影机，以及如何通过将它们附加到节点来将它们集成到场景中。本节将解释这两种类型的摄像机之间的区别，以及摄像机的一般处理方法。

## 透视和正交相机

摄像机有两种：*观点*摄像机，其中观察体积是一个截短的棱锥体（通常称为“视锥体”），以及*正字法*摄影机，其中的观察体积是一个矩形框。主要区别在于使用*观点*摄影机会导致正确的透视失真，而使用*正字法*照相机可以保持长度和角度。

中的示例[简单的照相机](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_015_SimpleCameras.md)节包含每种类型的一个摄影机、索引0处的透视摄影机和索引1处的正交摄影机：

```json
"cameras" : [
  {
    "type": "perspective",
    "perspective": {
      "aspectRatio": 1.0,
      "yfov": 0.7,
      "zfar": 100,
      "znear": 0.01
    }
  },
  {
    "type": "orthographic",
    "orthographic": {
      "xmag": 1.0,
      "ymag": 1.0,
      "zfar": 100,
      "znear": 0.01
    }
  }
],

```

这个`type`可以把它作为摄像机的一个字符串`“透视图”`或`"orthographic"`. 根据这种类型`照相机`对象包含[`camera.perspective`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-perspective)对象或[`camera.orthographic`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-orthographic)对象。这些对象包含定义实际查看体积的附加参数。

这个`camera.perspective`对象包含`相貌比`属性，该属性定义视口的纵横比。此外，它还包含一个名为`yfov`，代表 *Y方向视野*. 它定义相机的“打开角度”，以弧度表示。

这个`camera.orthographic`对象包含` xmag公司`和`ymag`属性。这些定义了相机在x和y方向的放大倍数，基本上描述了观察体积的宽度和高度。

两种相机类型都额外包含`znear`和` zfar公司`属性，即近剪裁平面和远剪裁平面的坐标。对于透视相机`zfar`值是可选的。当它丢失时，将使用一个特殊的“无限投影矩阵”。

解释相机、视图和投影的详细信息超出了本教程的范围。重要的一点是，大多数图形api都提供了直接基于这些参数定义查看配置的方法。通常，这些参数可用于计算*摄像机矩阵*. 摄像机矩阵可以倒置，以获得*视图矩阵*，稍后将与*模型矩阵*获得*模型视图矩阵*，这是渲染器所需的

# 摄像机方位

A`camera`可以变换为在场景中具有一定的方向和观看方向。这是通过将相机连接到`节点`. 每个[`node`](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0/#reference-node)可能包含`照相机`那是附属于它的。在简单的摄影机示例中，摄影机有两个节点。第一个节点是索引为0的透视摄影机，第二个节点是索引为1的正交摄影机：

```
"nodes" : {
  ...
  {
    "translation" : [ 0.5, 0.5, 3.0 ],
    "camera" : 0
  },
  {
    "translation" : [ 0.5, 0.5, 3.0 ],
    "camera" : 1
  }
},

```

如图所示[场景和节点](scene-and-nodes.md)节中，这些节点可能具有定义节点的变换矩阵的属性。这个[全局变换](scene-and-nodes.md#global-transforms-of-nodes)然后定义场景中摄影机的实际方向。可以任意应用[动画](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_007_Animations.md)对于节点，甚至可以定义摄影机飞行。

当摄像机节点的全局变换为单位矩阵时，摄像机的视点在原点处，观察方向沿负z轴。在给定的示例中，节点都具有`translation`关于` (0.5, 0.5, 3.0)`，这将导致摄影机相应地进行变换：它在x和y方向平移约0.5，以查看单位正方形的中心，并沿z轴平移约3.0，使其稍微远离对象。

## 相机实例与管理

glTF的JSON部分可能定义了多个摄像机。每个摄像机可以被多个节点引用。因此，glTF资源中显示的摄影机实际上是实际摄影机的“模板”*实例*：每当一个节点引用一个摄影机时，就会创建该摄影机的新实例。

glTF资源没有“默认”摄影机。相反，客户端应用程序必须跟踪当前活动的摄像机。例如，客户端应用程序可以提供一个下拉菜单，允许用户选择活动摄像机，从而在预定义的视图配置之间快速切换。通过更多的实现工作，客户机应用程序还可以为摄像机控件定义自己的摄像机和交互模式（例如，用鼠标滚轮缩放）。然而，在这种情况下，导航和交互的逻辑必须由客户机应用程序单独实现。[图15a](https://github.com/KhronosGroup/glTF-Tutorials/blob/master/gltfTutorial/gltfTutorial_015_SimpleCameras.md#cameras-png)显示了这种实现的结果，用户可以从glTF资源中定义的活动摄影机中选择活动摄影机，也可以选择可以用鼠标控制的“外部摄影机”。