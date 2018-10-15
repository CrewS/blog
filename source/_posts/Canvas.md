---
title: Canvas
date: 2018-10-11 15:56:08
tags:
- Document
- Canvas
---

通过使用javascript中的脚本来绘制图形。用于绘制图形，制作图片，创建动画，甚至进行实时视频处理或渲染。

### 1. canvas元素
```
  <canvas id=”tutorial” width=”150” height:”150”></canvas>
  
  // 渲染上下文
  Var canvas = document.getElementById(‘tutorial’);
  Var ctx = canvas.getContext(‘2d’)

  // 检查支持性
  If (canvas.getContext)
```
<br>

### 2. 绘制
- *绘制矩形*
canvas只支持一种原生的图形绘制：矩形。所有其他的图形的绘制都至少需要生成一条路径。
```
ctx.fillRect(x, y ,width, height)     绘制一个填充的矩形
ctx.strokeRect(x, y, width, height)   绘制一个矩形的边框
ctx.clearRect(x, y, width, height)    清除指定矩形区域，让清除部分完全透明
```

- *绘制路径*
路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。
首先，你需要创建路径或起点。然后使用画图命令去画出路径。把路径封闭。一旦路径生成，通过描边或填充路径区域来渲染图形。
```
ctx.beginPath()                                  新建路径 
ctx.closePath()                                  闭合路径
ctx.stroke()                                     绘制图形轮廓
ctx.fill()                                       填充路径内容区域
ctx.lineTo(x, y)                                 绘制一条从x到y的直线
ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise)绘制圆弧或者圆
ctx.quadraticCurveTo(cp1x, cp1y, x, y)           绘制二次贝塞尔曲线
ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)  绘制三次贝塞尔曲线
ctx.rect(x, y ,width, height)                    绘制一个左上角坐标为(x, y)的矩形
```

- *渲染图形*
6. 色彩
fillStyle = color      设置图形的填充颜色
strokeStyle = color   设置图形轮廓的颜色

7.线型
lineWidth = value       线条宽度
lineCap = butt/round/square   线条末端样式
lineJoin = type          线条与线条间接合处的样式
miterLimit = value  
getLineDash()          返回一个包含当前虚线样式，长度为非负偶数的数组
setLineDash(segments)  设置当前虚线样式
lineDashOffset = value   设置虚线样式的起始偏移量

8. 渐变
用线性或者径向的渐变来填充或描边，新建一个canvasGradient对象，并赋给图形的fillStyle或strokeStyle属性。
var lineargradient = ctx.createLinearGradient(x1, y1, x2, y2)
Var radialgradient = ctx.createRadialGradient(x1, y1, r1, x2, y2, r2)
Llineargradient.addColorStop(position, color)  // position是0到1之间的数值，表示渐变中颜色所在的相对位置 

9. 图案样式
createPattern(image, type) // type为repeat, repeat-x, repeat-y, no-repeat

var img = new Image();
  img.src = 'images/wallpaper.png';
  img.onload = function(){
    // 创建图案
    var ptrn = ctx.createPattern(img,'repeat');
    ctx.fillStyle = ptrn;
    ctx.fillRect(0,0,150,150);
  }

10. 阴影
11. 绘制文本
fillText(text, x, y [,maxWidth])     在指定位置填充指定文本
strokeText(text, x, y [,maxWidth])  在制定位置绘制文本边框

ctx.font = "48px serif";
ctx.strokeText("Hello world", 10, 50);

文本样式
Font =10px sans-serif
textAlign = start, end, left, right or center
textBaseline = top, hanging, middle, alphabetic, ideographic, bottom
Direction = ltr, rtl, inherit

12. 绘制图片
Var img = new Image();
Img.onload = function(){
Ctx.drawImage(img, 0, 0)
}
Img.src=’images/backdrop.png’

缩放图片
drawImage(image, x, y, width, height)

切片
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
 

13. 变形 transformations
save()和restore()是用来保存和恢复canvas状态，都没有参数。Canvas的状态就是当前画面应用的所有样式和变形的一个快照。变形前translate(x, y)是一个良好习惯，调用restore方法比手动恢复原先的状态要简单得多。










===
WhiteBoard
Export default class WhiteBoard{
Constructor(options){
This.canvas
This.ctx
This.isdrawing
This.strokeColor
This.ctx.lineWidth
This.strokes = [
{
data: [{x, y}, {x, y}],
strokeColor:
width:
graphType: pencil笔画/ line直线 / circle圆形 / square 矩形 / rubber 橡皮檫
}
]
This.graphType
This.socket
This.redo = []
This.init()
}

Init = () => {
// 绑定事件：鼠标点击、鼠标移动、鼠标抬起
Canvas.onmousedown = () => { this.addPoint(event.clientX - offsetLeft, event.clientY - offsetTop, true)}
Canvas.onmousemove = () => {
If (this.isDrawing){ this.addPoint(x,y) }  // 描点
If( this.graphType == ‘rubber’ )       //  橡皮檫，白色画笔
}
Canvas.onmouseup = () => {
this.paint(canvas, ctx, current)
this.socket.send( JSON.stringify({ kind: MESSAGE_STROKE, pointes: current, storke_kind: 0))
}
Canvas.onmouseleave = () => {
This.isDrawing = false
}
}

// 定义绘制事件
Update(
// 清空再重绘
this.drawStrokes(this.strokes)
)  
addPoint(x, y, newStroke)
drawStrokes = (strokes) => {
// 绘制行为
}
repaint = () => {
// 对撤销和清空的行为进行重绘
}
paint(canvas, ctx, stroke) {
// 画在画布上，也是客户端接收数据时候的paint行为
} 
clear = () => {
// 清空
this.socket.send(JSON.stringify({ kind: MESSAGE_STROKE, points:[], stroke_kind:1 }))
}
cancelOneStep () => {
// 撤销
this.redo.push(this.strokes.pop())
this.socket.send(JSON.stringify({ kind: MESSAGE_STROKE, points:[], stroke_kind:2 }))
}
redoStep () => {
// 重做
this.strokes.push(this.redo.pop())
this.socket.send(JSON.stringify({ kind: MESSAGE_STROKE, points:[], stroke_kind:3 }))

}


// 样式方法
setLineWidth
setRubber
setGraphType
setGraphColor
}

ClientBoard
Export class ClientBoard{
Constructor(){
this.canvas
this.ctx
this.socket = new WebSocket(ws)
this.socket.onmessage = (event) => {
Messages.map((data) =>{
this.onMessage(JSON.parse(data))
})
}
this.socket.onopen
this.strokes
this.redo
}
}

Q:
 paint和drawStrokes




