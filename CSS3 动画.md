# CSS3 动画

## 3 种动画方式

- translate 过渡
- transform 转换
- animation 动画





## 一、translate 过渡

> transition 是一个简写属性，用于设置四个过渡属性：

1. `transition-property` ：规定需要产生过渡效果的 CSS 属性名，当给多个属性分别添加时使用逗号隔开

2. `transition-duration` ： 规定完成过渡效果需要多少秒或毫秒

3. `transition-timing-function` ：规定速度效果的速度曲线
   - `linear` ：规定以相同速度开始至结束的过渡效果
   - `ease` ：规定慢速开始，然后变快，然后慢速结束的过渡效果
   - `ease-in` ：规定以慢速开始的过渡效果
   - `ease-out` ：规定以慢速结束的过渡效果
   - `ease-in-out` ：规定以慢速开始和结束的过渡效果

4. `transition-delay` ：规定过渡效果何时开始

> 简写：

- 语法： `transition: property duration timing-function delay;`
- 默认值：`transition: all 0 ease 0;`
- eg： `transition: background-color 2s linear 1s;`
- 当给多个属性添加过渡效果时用逗号隔开：
  举例：`transition: width 2s ease, background-color 1.5s 0.8s;`

> 兼容性：

1. IE10 以及以上版本才支持 translate
2. 其他浏览器也需要不同版本才可以实现

> 在vue中如何使用 transition

Vue 提供了 transition 的封装组件，用来包裹需要在切换添加过渡动画的组件或元素。



## 二、transform 转换

> transform 属性可以实现元素的 2D 或 3D 转换

该属性允许我们对元素进行旋转、缩放、移动、倾斜等基本动画操作：

> 语法：transform: none|transform-functions;

1. none： 不进行转换（默认）

2. transform-functions：

   - translate 移动

     - `translate(x,y)`	移动x轴、y轴多少距离
     - `translate3d(x,y,z)`	移动x轴、y轴、z轴多少距离（3D转换）
     - `translateX(x)`	只移动x轴多少距离
     -  `translateY(y)`	只移动y轴多少距离
     - `translateZ(z)`	只移动z轴多少距离（3D转换）

   - scale(x,y)	定义 2D 缩放转换

     - `scale3d(x,y,z)`	定义 3D 缩放转换
     - `scaleX(x)`	通过设置 X 轴的值来定义缩放转换
     - `scaleY(y)`	通过设置 Y 轴的值来定义缩放转换
     - `scaleZ(z)`	通过设置 Z 轴的值来定义 3D 缩放转换

   - rotate 旋转（<number> deg，正数顺时针，负数逆时针）

     - `rotate(angle)`	定义 2D 旋转，在参数中规定角度
     - `rotate3d(x,y,z,angle)`	定义 3D 旋转
     -  `rotateX(angle)`	定义沿着 X 轴的 3D 旋转
     - `rotateY(angle)`	定义沿着 Y 轴的 3D 旋转
     -  `rotateZ (angle)`	定义沿着 Z 轴的 3D 旋转

   - skew 倾斜（<number> deg）

     - `skew(x-angle,y-angle)`	定义沿着 X 和 Y 轴的 2D 倾斜转换

     - `skewX(angle)`	定义沿着 X 轴的 2D 倾斜转换

     - `skewY(angle)`	定义沿着 Y 轴的 2D 倾斜转换

     - `perspective(n)`	为 3D 转换元素定义透视视图

> transform-origin：设置转换参考点，默认参考点为元素中心点

1. 默认: `transform-origin:50% 50% 0;`
2. 语法: `transform-origin: x-axis y-axis z-axis;`
3. transform-origin 属性值既可以是 %、em、px 等具体的值，也可以是 top、right、bottom、left 和 center 

>  transform-style：设置元素的子元素是位于 3D 空间中还是平面中

如果选择平面 flat，元素的子元素将不会有 3D 的遮挡关系

1. `transform-style: flat;`
2. `transform-style: preserve-3d;`



## 三、animation动画

> animation 动画有8个属性：

1. `animation-name: name;`  设置动画名字，每个名称代表一个由@keyframes定义的动画序列

2. `animation-duration: 2s;`  定义动画完成一个周期需要多少秒或毫秒

3. `animation-timing-function: ease; `  决定动画将以什么样的速度执行，值可以为一个或多个
   - `ease` ：动画以低速开始，然后加快，在结束前变慢(默认)
   - `linear` ：动画从头到尾的速度是相同的
   - `ease-in` ： 动画以低速开始
   - `ease-out` ：动画以低速结束
   - `ease-in-out` ：动画以低速开始和结束
4. `animation-delay: 2s;`  定义动画延迟执行时间

5. `animation-iteration-count: 1;`  定义动画执行次数，可以是1次，也可以无限循环。默认执行1次

   如果指定了多个值，每次播放动画时，将使用列表中的下一个值，在使用最后一个值后循环会第一个

   - `infinite` ： 无限循环播放动画
   - `<number>` ：动画播放的次数，也可以用小数

6. `animation-direction: alternate;`  定义是否循环交替反向播放动画

   如果动画被设置为只播放一次，该属性将不起作用

   - `normal` ：正向运行动画，动画播放一次结束，动画重置到起点位置重新开始
   - `reverse` ：反向运行动画，动画播放一次结束，动画重置到结束位置重新开始
   - `alternate` ：动画在奇数次（1、3、5…）正向播放，在偶数次（2、4、6…）反向播放
   - `alternate-reverse` ：动画在奇数次（1、3、5…）反向播放，在偶数次（2、4、6…）正向播放

7. `animation-fill-mode: forwards;`   定义动画不播放时或者动画播放结束时，要应用到元素的样式
   - `none` ：当动画未执行时，动画将不会将任何样式应用于目标（默认）
   - `forwards` ：动画完成后，元素状态保持为最后一帧的状态
   - `backwards` ：在动画延迟执行期间应用第一个关键帧中定义的值
   - `both` ：同时应用 forwards 和 backwards 的效果

8. `animation-play-state: running`   指定动画是否正在运行或已暂停
   - `paused`：指定暂停动画
   - `running`：指定正在运行的动画

> animation的简写（即上述属性的介绍顺序）

1. 建议书写顺序：

   `animation: name duration timing-function delay iteration-count direction fill-mode play-state;`

2. 举例：`animation:myAnim 1s linear 1s infinite alternate both running;`

> keyframes：定义动画规则，关键帧

1. 举例：

   ~~~CSS
   @keyframes myAnim {
     0% { background: #f00; }
     50% { background: #0f0; }
     100% { background: yellowgreen; }
   }
   ~~~

> 注意事项：

- 定义动画时，必须定义动画的名称和动画的持续时间

- 如果省略持续时间，动画将无法运行，因为默认值是 0



## 四、补充知识点：透视

perspective（透视）：指定了眼睛与translateZ（0）的距离

> 作用：

1. 其中属性值 > 0，则看起来比正常盒子大（但实际值并没有改变，只是看起来大）;

2. 属性值 < 0，则看起来比正常盒子小（同样实际值没有改变，只是看起来小了一点）。

> 举例：

1. 如果想创建特别深的视域，仿照变焦镜头的效果，可以声明 `perspective: 2500px;`
2. 如果想让深度浅一些，模仿鱼眼镜头的近景效果，可以声明 `perspective: 200px;`



## 五、动画相关的踩坑记录

### 1. 消除父级元素动画对子元素的影响

内部盒子主要通过设置一个和外部盒子动画反方向的动画来抵消外部动画对内部的影响

> 示例：

~~~CSS
/* 外部盒子 */
.outer {
  animation: outerRotate 10s linear infinite;
}

/* 内部盒子容器：用于抵消父元素动画对子元素的影响*/
.wrapper {
  animation: outerRotate1 10s linear infinite;
}

/* 内部盒子 */
.inner {
  animation: innerRotate 5s linear infinite;
}

/* 外部盒子旋转动画 */
@keyframes outerRotate {
  0% {
    transform: rotateY(360deg) rotateX(0deg)
  }
  50% {
    transform: rotateY(0deg) rotateX(90deg)
  }
  100% {
    transform: rotateY(-180deg) rotateX(0deg)
  }
}

/* 外部盒子容器旋转动画 */
@keyframes outerRotate1 {
  0% {
    transform:rotateX(0deg) rotateY(-360deg) 
  }
  50% {
    transform:rotateX(-90deg) rotateY(0deg) 
  }
  100% {
    transform: rotateX(0deg) rotateY(180deg) 
  }
}

/* 内部盒子旋转动画 */
@keyframes innerRotate {
  0% {
    transform: rotateY(360deg);
  }
  100% {
    transform: rotateY(0deg);
  }
}


<!-- 外部盒子 -->
<div class="outer">

   <!-- 内部盒子容器，用于抵消父元素动画对子元素的影响 -->
   <div class="wrapper">

     <!-- 内部盒子 -->
     <div class="inner"></div>

   </div>

</div>
~~~

