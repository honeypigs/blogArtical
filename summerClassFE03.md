
## 像素(A pixel is not a pixel is not a pixel)

### 设备像素

> 屏幕的物理像素，任何设备屏幕的物理像素的数量都是固定不变的，单位是pt。
> 也就是设备的分辨率: 1080\*1920 1378\*786   
> 也就是设备的分辨率: 1080x1920 1378x786   


+ PPI
> Pixels Per Inch 像素密度 (pixel density)单位，`dpi`(dot per inch)即每英寸的長度中所具有的像素。

它决定了屏幕显示东西是否细腻,ppi太低了会屏幕会有颗粒感,所以现在很多手机厂商做的超高分辨率的手机(但是300ppi以上肉眼基本无法分辨了)

### css像素

> 在 CSS、JS 中使用的一个抽象的概念，单位是 px。
> CSS 像素也可以称为设备独立像素 (device-independent pixels)，简称为dips，单位是dp.

获取一个设备的css像素 `screen.width`;


### 设备像素比率 （Device Pixel Ratio，DPR）

dpr = 物理像素/css像素

举个例子: ...

获取一个设备的dpr`window.devicePixelRatio`

### 物理像素与css像素关系
![](http://www.quirksmode.org/mobile/pix/viewport/csspixels_100.gif)

![](http://www.quirksmode.org/mobile/pix/viewport/csspixels_out.gif)

![](http://www.quirksmode.org/mobile/pix/viewport/csspixels_in.gif)

## viewport

`<meta name="viewport" content="width=980">`

### pc上的概念

> viewport 的定义是严格的等于浏览器的窗口，它不是一个 HTML 的概念，所以我们不能通过 css 去修改它。
> 实际上，viewport 的功能在于控制网站的最高 block 容器，那就是 <html> 元素。也就是说 <html> 元素为 viewport 宽度的 100%。虽然我们知道document.documentElement实际上就是<html>元素，但是document.documentElement. clientWidth/Height给出的实际是 viewport 的尺寸，而不是<html>元素，不管 <html> 元素的尺寸如何改变，都只会得到 viewport 的尺寸。那我们该如何获取 <html> 元素的尺寸呢，可以使用 document.documentElement.offsetWidth/Height，但还是有 bug，IE 用这个值标示 viewport 的尺寸而非<html>

+ **有啥用**
	+ 事件坐标
		
		1.pageX/Y给出的坐标相对于<html> 元素的CSS像素。
		
		2.clientX/Y 给出的坐标相对于视在CSS像素。
		
		3.screenX/Y 给出的坐标相对于屏幕的设备像素。 
		
		4.layerX/Y 给出的是坐标相对与监听元素的css像素

### 移动端概念

+ 视觉视口 (visual viewport )
> visualviewport课简单认为是物理可视化区域。用于承载布局视口。screen.Width/Height 返回 visualviewport 的尺寸。

+ 布局视口 (layout viewport)
> 相当于PC端的视口，css 布局通常都是按照 layoutviewport 来定义，因此比 visualviewport 宽很多。。每个浏览器的布局视口 都不同。iPhone 上的 Safari 使用 980px、Opera 850px，安卓的 Webkit 核心 800px，IE974px。浏览器已经选择好他们的layoutviewport的尺寸，它完整的覆盖了最小缩放模式下的移动浏览器的屏幕。document.documentElement.clientWidth/Height传递layoutviewport的尺寸。

+ 完美视口(ideal viewport)
>我们还需要一个视口，它类似于布局视口，但宽度和视觉视口相同，这就是完美视口（。
有了完美视口，用户不用缩放和拖动网页就能够很好的进行网页浏览。而完美视口也是通过viewport meta的某种设置来达到。
	
### viewport的 width 和 device-width
> `width/height`使用以CSS的pixels度量的`layoutviewport`，通俗的说就是页面内容的宽度 , 反映 
`document.documentElement.clientWidth/Height`的值；
> `device-width/height`使用以设备的pixels度量的设备屏幕`device screen` ,反映screen.width/height；

## 几个mate的content值
|Name	        |   Value	           |  Description            |
| ------        | ------------------   | :---------------------: |
|width	        | 正整数或device-width  |	 定义视口的宽度，单位为像素 |
|height	        | 正整数或device-height |	 定义视口的高度，单位为像素 |
|initial-scale	| [0.0-10.0]           | 定义初始缩放值 |
|minimum-scale	| [0.0-10.0]   	       | 定义缩小最小比例，它必须小于或等于maximum-scale设置 |
|maximum-scale	| [0.0-10.0]	           | 定义放大最大比例，它必须大于或等于minimum-scale设置 |
|user-scalable	| yes/no	           | 定义是否允许用户手动缩放页面，默认值yes |

## 媒体查询
> 媒体查询是可应用于 CSS 样式的简单过滤器。有了这些过滤器，我们可以根据设备呈现内容的特点轻松更改样式，包括显示屏类型、宽度、高度、方向甚至是分辨率。


> 媒体查询可用于根据设备特点应用样式。


包含媒体类型和媒体属性

逻辑操作符
and not only

```css
@media (query) {
  /* CSS Rules used when query matches */
}
```
查询语句可以写多条
```css
@media (query) and (query) {
  /* CSS Rules used when query matches */
}
```


| 属性                   |            结果            |
| --------------------- | :----------------------: |
| min-width             | 当任意浏览器宽度大于查询中定义的值时适用的规则  |
| max-width             | 当任意浏览器宽度大于查询中定义的值时适用的规则。 |
| min-height            | 当任意浏览器高度大于查询中定义的值时适用的规则。 |
| max-height            | 当任意浏览器高度小于查询中定义的值时适用的规则。 |
| orientation=portrait  |  高度大于或等于宽度的任意浏览器适用的规则。   |
| orientation=landscape |    宽度大于高度的任意浏览器适用的规则     |

更多高级的用法: https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries




**典型移动端优化写法**

`width=device-width,initial-scale=1.0,minimum-scale=1.0, maximum-scale=1.0,user-scalable=no`


## 新单位rem
> rem就是相对于根元素<html>的font-size来做计算

也就是说如果html 的font-size是12,一个div的width是1rem,那么这个div的width就是12px;

**于是,我们可以想到两种移动端的布局方案**

## 宽度固定viewport缩放
> 视觉稿、页面宽度、viewport width 使用统一宽度，利用浏览器自身缩放完成适配。页面样式（包括图像元素）完全按照视觉稿的尺寸，使用定值单位 （px、em）即可完成。

优点

+ 开发简单
+ 还原精准
+ 测试方便

缺点

+ 像素丢失
  对于一些分辨率较低的手机，可能设备像素还未达到指定的 viewport 宽度，此时屏幕的渲染可能就不准确了。比较常见的是边框 “消失” 了，不过随着手机硬件的更新，这个问题会越来越少的

+ 缩放失效 
  某些安卓机不能正常的根据 meta 标签中 width 的值来缩放 viewport，需要配合 initial-scale

+ 文本折行
  存在于缩放失效的机型中，某些手机为了便于文本的阅读，在文本到达 viewport 边缘（非元素容器的边缘）时即进行折行，而当 viewport 宽度被修正后，浏览器并没有正确的重绘，所以就发现文本没有占满整行。一些常用的段落性文本标签会存在该问题。



## rem布局
依照某特定宽度设定 rem 值（即 html 的 font-size），页面任何需要弹性适配的元素，尺寸均换算为 rem 进行布局；当页面渲染时，根据页面有效宽度进行计算，调整 rem 的大小，动态缩放以达到适配的效果。利用该方案，还可以根据 devicePixelRatio 设定 initial-scale 来放大 viewport，使页面按照物理像素渲染，提升清晰度。

优点
+ 清晰度高，能达到物理像素的清晰度。
+ 向后兼容较好，即便屏幕宽度增加、PPI 增加该方案依旧适用。

缺点
+ 某些 Android 机会丢掉 rem 小数部分。
+ 需要转换单位

# px2rem 把px转换成rem的插件 https://www.npmjs.com/package/px2rem

+ rem布局参考 https://github.com/amfe/article/issues/17




## 参考资料
+ http://www.cnblogs.com/skylar/p/mobile.html
+ https://segmentfault.com/a/1190000002546941
+ http://www.cnblogs.com/2050/p/3877280.html
+ http://webdesign.tutsplus.com/zh-hans/articles/quick-tip-dont-forget-the-viewport-meta-tag--webdesign-5972
+ https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries
+ http://html-js.com/article/MobileWeb
+ https://github.com/riskers/blog/issues/18
+ http://www.cnblogs.com/lyzg/p/4877277.html
