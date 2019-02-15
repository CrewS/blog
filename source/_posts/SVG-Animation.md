---
title: SVG Animation, SVG spi
date: 2018-10-05 16:28:15
tags:
- CSS
category:
- CSS
---
### 1. SVG Animation
Animation is done by manipulating the attributes of shapes over time. This is done using one or more of the 5 SVG animation elements:`<set> <animate> <animateColor> <animateTransform> <animateMotion>`

#### 1.1 *`<set>`*
The shape is not continuously animating, but just changes attribute attribute value once after a specific time. The example above sets the attribute `r` to 100 after 5 seconds.
```
<svg width="500" height="100" version="1.1">
  <circle cx="30" cy="30" r="25" style="fill: #0000ff;">
    <set attributeName="r" attributeType="XML" to="100" begin="5s"/>
  </circle>
</svg>
```


#### 1.2  *`<animate>`*
The `animate` element is used to animate an attribute of an SVG shape. You nest the `animate` element inside the shape you want it applied to. 
```
<circle cx="30" cy="30" r="25" style="stroke: none; fill: #0000ff;">
  <animate attributeName="cx" attributeType="XML"
    from="30"  to="470"
    begin="0s" dur="5s"
    fill="remove" repeatCount="indefinite"
  />
</circle>
```
This example animate the `cx` attribute of the `<circle>` element from a value of 30 to the value of 470. The animation starts at 0 seconds, and has a duration of 5 seconds. The animation repeats indefinitely.


#### 1.3  *`<animateTransform>`*
The `<animateTransform>` element can animate the `transform` attribute of a shape.

The example animates the `transform` attribute of the `<rect>` element it is nested inside. The `type` attribute is set to `rotate` meaning the animated transformation will be a rotation. The `from` and `to` attributes set the parameters to be animated and passed to `rotate` function. This example rotates from a degree of 0 to a degree of 360 around point 5,5.
<br>

{% raw %}
<svg width="50" height="50">
  <rect x="10" y="10" width="10" height="10"style="stroke: #ff00ff; fill:none;" >
    <animateTransform attributeName="transform"
      type="rotate"
      from="0 5 5" to="360 5 5"
      begin="0s" dur="10s"
      repeatCount="indefinite"
    />
  </rect>
</svg>
{% endraw %}

```
<rect x="10" y="10" width="10" height="10"
    style="stroke: #ff00ff; fill:none;" >
  <animateTransform attributeName="transform"
    type="rotate"
    from="0 5 5" to="360 5 5"
    begin="0s" dur="10s"
    repeatCount="indefinite"
  />
</rect>

<rect x="20" y="20" width="40" height="40" style="stroke: #ff00ff; fill: none;" >
    <animateTransform attributeName="transform"
      type="scale"
      from="1 1" to="2 3"
      begin="0s" dur="10s"
      repeatCount="indefinite"
    />
</rect>
```
<br>

#### 1.4 *`<animateMotion>`*
The `<animateMotion>` element can animate the movement of a shape along a path.

In order to rotate the square to align with the slope of the path, you can set the `rotate` attribute of the `<animateMotion>` element to `auto`. You can also set the `rotate` attribute to a specific value, like 20 or 30 etc. That will keep the shape rotated that number of degrees throughout the animation.
```
<svg width="500" height="100" version="1.1" xmlns="http://www.w3.org/2000/svg">
    <rect x="0" y="0" width="30" height="15" style="stroke: #ff0000; fill: none;">
        <animateMotion path="M10,50 q60,50 100,0 q60,-50 100,0" begin="0s" dur="10s" repeatCount="indefinite" rotate="auto"/>
    </rect>
</svg>
```
<img src="1.jpeg" style="max-width: 380px">
<br>


#### 1.5 Time Units
h, min, s, ms, hh:mm:ss
<br>


#### 1.6 Coordinating Animations
The `begin` attribute value is set to `one.end` which means that this animation should start when the animation with the id `one` ends. You can also specify offsets to start or end time of another animation, like `one.begin+10s`.
```
<rect x="0" y="0" width="30" height="15" style="stroke: #ff0000; fill: none;">
  <animate id="one" attributeName="x" attributeType="XML"
    from="0" to="400"
    begin="0s" dur="10s" fill="freeze"
  />
  <animate attributeName="y" attributeType="XML"
    from="0" to="50"
    begin="one.end" dur="10s" fill="freeze"
    />
</rect>
```
<br>

#### 1.7 Repeating Animations
There are two attributes you can use inside an animation element which are used to repeat the animation. 

The first attribute is the `repeatCount` attribute. You can set a number of times, or the value `indefinite` which will keep the animation running without ever stopping.

The second attribute is the `repeatDur` which specifies a duration for which the animation is to be repeated.
```
<animate attributeName="y" attributeType="XML"
  from="0" to="50"
  begin="one.end" dur="10s" fill="freeze"
  repeatCount="3"
  // repeatCount="indefinite"
  // repeatDur="30s"
  />
```
<br>

#### 1.8 Combining Animations
You can combine animations by listing more than one `<animation>` inside the element to animate. 

When combining `<animateTransform>` elements, the default behaviour is for the second animation to cancel out the first. Howeve, you can combine the transformation animations by add the attribute `additive` with a value of `sum` to both `<animateTransform>` elemens. 

Here is an example which both scales and rotates a rectangle.
```
<rect x="10" y="10" width="40" height="20" style="stroke: #000000; fill: none;">
    <animateTransform attributeName="transform" attributeType="XML"
      type="scale"
      from="1" to="3"
      begin="0s" dur="10s"
      repeatCount="indefinite"
      additive="sum"
  />
    <animateTransform attributeName="transform" attributeType="XML"
      type="rotate"
      from="0 30 20" to="360 30 20"
      begin="0s" dur="10s"
      fill="freeze"
      repeatCount="indefinite"
      additive="sum"
  />
</rect>
```
<br>


### 2. SVG Scripting
Via scripting you can modify the SVG elements, animate them, or listen for mouse events on the shapes.


#### 2.1 Changing attribute values
Here is a simple SVG scripting example which changes the dimensions of an SVG rectangle when a button is clicked.

{% raw %}
<svg width="500" height="100">
    <rect id="rect1" x="10" y="10" width="50" height="80" style="stroke:#000000; fill:none;"/>
</svg>
<input id="button1" type="button" value="Change Dimensions" onclick="changeDimensions()"/>
<script>
  function changeDimensions() {
    document.getElementById("rect1").setAttribute("width", "100");
  }
</script>
{% endraw %}

```
<svg width="500" height="100">
    <rect id="rect1" x="10" y="10" width="50" height="80" style="stroke:#000000; fill:none;"/>
</svg>
<input id="button1" type="button" value="Change Dimensions" onclick="changeDimensions()"/>
<script>
  function changeDimensions() {
    var svgElement = document.getElementById("rect1");
    var width = svgElement.getAttribute("width");
    svgElement.setAttribute("width", "100");
  }
</script>
```
<br>


#### 2.2 Event Listeners
You can add event listeners to an SVG shape directly in the SVG if you want. You do so just like you would with an HTML element. You can also attach an event listener to an SVG element using the `addEventListener()` function.
```
<rect x="10" y="10" width="100" height="75" style="stroke: #000000; fill: #eeeeee;"
  onmouseover="this.style.stroke = '#ff0000'; this.style['stroke-width'] = 5;"
  onmouseout="this.style.stroke = '#000000'; this.style['stroke-width'] = 1;"
/> 
```

{% raw %}
<svg width="500" height="100">
  <rect x="10" y="10" width="100" height="75" style="stroke: #000000; fill: #eeeeee;"
    onmouseover="this.style.stroke = '#ff0000'; this.style['stroke-width'] = 5;"
    onmouseout="this.style.stroke = '#000000'; this.style['stroke-width'] = 1;"
  /> 
</svg>
{% endraw %}
<br>

#### 2.3 Animating SVG Shapes
In order to animate an SVG shape you need to call a Javascript function repeatedly. The function changes the position or dimensions of a shape.
{% raw %}
<svg width="500" height="100">
    <circle id="circle1" cx="20" cy="20" r="10"
            style="stroke: none; fill: #ff0000;"/>
</svg>

<script>
    var timerFunction = null;

    function startAnimation() {
        if(timerFunction == null) {
            timerFunction = setInterval(animate, 20);
        }
    }

    function stopAnimation() {
        if(timerFunction != null){
            clearInterval(timerFunction);
            timerFunction = null;
        }
    }

    function animate() {
        var circle = document.getElementById("circle1");
        var x = circle.getAttribute("cx");
        var newX = 2 + parseInt(x);
        if(newX > 500) {
            newX = 20;
        }
        circle.setAttribute("cx", newX);
    }
</script>
<br/>
<input type="button" value="Start Animation" onclick="startAnimation();">
<input type="button" value="Stop Animation" onclick="stopAnimation();">
{% endraw %}

```
<svg width="500" height="100">
    <circle id="circle1" cx="20" cy="20" r="10"
            style="stroke: none; fill: #ff0000;"/>
</svg>
<script>
    var timerFunction = null;

    function startAnimation() {
        if(timerFunction == null) {
            timerFunction = setInterval(animate, 20);
        }
    }

    function stopAnimation() {
        if(timerFunction != null){
            clearInterval(timerFunction);
            timerFunction = null;
        }
    }

    function animate() {
        var circle = document.getElementById("circle1");
        var x = circle.getAttribute("cx");
        var newX = 2 + parseInt(x);
        if(newX > 500) {
            newX = 20;
        }
        circle.setAttribute("cx", newX);
    }
</script>
<input type="button" value="Start Animation" onclick="startAnimation();">
<input type="button" value="Stop Animation" onclick="stopAnimation();">
```


### 3. SVG实现
#### 3.1 Loading icon
#### 3.2 Wave background


<br>
### 参考资料
- [SVG Tutorial](http://tutorials.jenkov.com/svg/svg-animation.html)
- [SVG 浪啊，浪来了，大浪来了](https://zhuanlan.zhihu.com/p/36031294)
- [Ant Design Icons](https://ant.design/components/icon-cn/)