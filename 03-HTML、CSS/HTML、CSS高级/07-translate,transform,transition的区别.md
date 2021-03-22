## 为什么会有本章的产生？

如果英文不是很好，又或者对这几个知识点使用较少、不熟悉，又或者长时间不使用后淡忘等。

对这三个知识点做一个简单的区别介绍，方便日常开发和复习。



### translate:移动，transform的一个方法

通过 translate() 方法，元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数：

用法transform: translate(50px, 100px);

-ms-transform: translate(50px,100px);

-webkit-transform: translate(50px,100px);

-o-transform: translate(50px,100px); 

-moz-transform: translate(50px,100px);

 

### transform:变形。改变

CSS3中主要包括 

- 旋转：rotate() 顺时针旋转给定的角度，允许负值 rotate(30deg)
- 扭曲：skew() 元素翻转给定的角度,根据给定的水平线（X 轴）和垂直线（Y 轴）参数：skew(30deg,20deg)
- 缩放：scale() 放大或缩小，根据给定的宽度（X 轴）和高度（Y 轴）参数： scale(2,4)
- 移动：translate() 平移，传进 x,y值，代表沿x轴和y轴平移的距离

所有的2D转换方法组合在一起： 

matrix()  旋转、缩放、移动以及倾斜元素  

 ```css
matrix(scale.x, scale.y, translate.x, translate.y)
 ```

改变起点位置 transform-origin: bottom left;



**transform: rotate旋转|scale缩放|skew扭曲|translate移动|matrix矩阵变形；**

综合起来使用：transform: 30deg 1.5 30deg 20deg 100px 200px;//需要有空格隔开



### transition: 允许CSS属性值在一定的时间区间内平滑的过渡

**transition作用**是指定了某一个属性（如width、left、transform等）在两个值之间如何过渡。



需要事件的触发，例如单击、获取焦点、失去焦点等

transition主要包含四个属性值：

**transition**: property duration timing-function delay;

(1) 执行变换的属性：transition-property;

(2) 变换延续的时间：transition-duration;

(3) 在延续时间段，变换的速率变化：transition-timing-function //例：平缓进入、先快后慢；

(4) 变换延迟时间： transition-delay
