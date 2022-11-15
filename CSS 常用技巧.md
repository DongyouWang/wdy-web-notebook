# CSS 常用技巧

## 浏览器

### 清除默认样式

[`normalize.css`](https://necolas.github.io/normalize.css)：懒人必备的浏览器默认样式，接近`40k`的`Star`，说明很多人都是懒人啦！

> 使用

在引入其他 `css文件` 前将其导入，通常在程序入口文件 `main.js` 中导入：

 ~~~JS
// main.js
...
import 'assets/css/normalize.css';
...
 ~~~

### 兼容性

**兼容性写法放到前面，标准性写法放到最后**

在浏览器解析 `CSS` 时，若标准属性无法使用，则使用当前浏览器相应 私有属性

浏览器私有属性：通常编写 `CSS` 时都会在一些 CSS3属性 前加入的前缀：

- `-webkit-`：Chrome、Safari、New Opera、New Edge
- ``-moz-`：Firefox
- `-ms-`：IExplorer、Old Edge
- `-o-`：Old Opera

**关于兼容 IE 浏览器的建议：** 不搞 IE 兼容，基本就无兼容问题

> 手动兼容：

~~~CSS
/* Chrome、Safari、New Opera、New Edge */
-webkit-transform: translate(10px, 10px);
/* Firefox */
-moz-transform: translate(10px, 10px);
/* IExplorer、Old Edge */
-ms-transform: translate(10px, 10px);
/* Old Opera */
-o-transform: translate(10px, 10px);

/* 标准 */
transform: translate(10px, 10px);
~~~

> 自动化兼容：利用 webpack 对 CSS 资源进行处理

每个 CSS3属性 都编写这么一堆兼容性代码，无疑是对生命最大的浪费！

1. 下载相关的 npm 包：

   ```console
   npm install --save-dev css-loader
   
   npm install --save-dev postcss-loader postcss
   ```

   `npm install --save-dev css-loader style-loader postcss-loader postcss postcss-preset-env`

   - `css-loader`：将通过 `@import` 和 `url()`  引入的 CSS 文件编译成 Webpack 能识别的模块内容
   - `style-loader`：把 CSS 插入到 DOM 中，创建一个 Style 标签，里面放置 Webpack 中 CSS 的模块内容
   - `postcss-loader`：用于进一步处理 CSS 文件
   -  `PostCSS` ：只是一个空壳，最大的优势在于其简单、易用、丰富的插件生态，比如添加浏览器前缀，压缩 CSS 、处理雪碧图等，经常使用的插件有：
     - [autoprefixer](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fpostcss%2Fautoprefixer)：基于 [Can I Use](https://link.juejin.cn/?target=https%3A%2F%2Fcaniuse.com%2F) 网站上的数据，自动添加浏览器前缀
     - [postcss-preset-env](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjonathantneal%2Fpostcss-preset-env)：将最新 CSS 新特性转译为兼容性更佳的低版本代码，它内置了 `autoprefixer`
     - [postcss-less](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fshellscape%2Fpostcss-less)：兼容 Less 语法的 `PostCSS` 插件，类似的还有：[postcss-sass](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FAleshaOleg%2Fpostcss-sass)、[poststylus](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmadeleineostoja%2Fpoststylus)
     - [stylelint](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fstylelint%2Fstylelint)：一个现代 CSS 代码风格检查器，能够帮助识别样式代码中的异常或风格问题
   - 

2. 在 webpack.config.js 文件中进行配置：

   ~~~JS
   module.exports = {
     // 打包文件入口：Webpack 会从这些入口文件开始按图索骥找出所有项目文件
     entry: "",
   
     // 出口，文件打包后
     output: {},
   
     // 用于配置模块加载规则，例如针对什么类型的资源需要使用哪些 Loader 进行处理
     // loader 加载器，或者叫代码转化器，用于将不同类型的文件转换为 webpack 可识别的模块,
     // 使得 Webpack 有能力去处理除了 JS、JSON 以外的其他类型的文件
       module: {
       rules: [
         {
           test: /\.css$/i,
           use: [
             'style-loader',
             {
               loader: 'css-loader',
               options: { importLoaders: 1 },
             },
             {
               loader: 'postcss-loader',
               options: {
                 postcssOptions: {
                   plugins: [
                     [
                       'postcss-preset-env',
                       {
                         // 选项
                       },
                     ],
                   ],
                 },
               },
             },
           ],
         },
       ],
     },
   
     // 插件，提供执行更广的任务的功能，包括：打包优化，资源管理，注入环境变量等
     plugins: [],
   
     // 模式，有开发和生产两种模式，根据不同运行环境执行不同构建打包方式
     mode: "development"
   };
   ~~~

## CSS 性能代码优化

### 1. 首屏加载时，内联关键CSS

性能优化中有一个重要的指标——指页面的首要内容出现在屏幕上的时间，和 首屏加载 概念类似

使用 link 外部CSS文件时，需要在HTML文档下载完成后才知道所要引用的CSS文件，然后才下载它们

将 CSS 直接内联到 HTML 文档中能使 CSS 更快速地下载，让浏览器开始页面渲染的时间提前

> 注意：

- 这种方式并不适用于内联较大的 CSS 文件(不超过 14.6 KB)，因此，我们应当只将渲染首屏内容所需的关键 CSS 内联到 HTML 中

- 缺点：内联之后的 CSS不会进行缓存，每次都会重新下载，建议剩余 CSS 使用外部 <link> 引入，这样能够启用缓存，除此之外还可以异步加载它们

------

### 2. 异步加载 CSS

CSS 会阻塞渲染，将首屏关键CSS内联后，剩余的CSS内容的阻塞渲染就不是必需的了，可以使用外部 CSS，并且开启异步加载，有以下四种方式可以实现浏览器异步加载 CSS：

1. 使用 JavaScript 动态创建样式表link元素，并插入到DOM中

   ~~~JS
   <script>
   const myCSS = document.createElement( "link" ); // 创建link标签
   myCSS.rel = "stylesheet";
   myCSS.href = "mystyles.css"; // CSS 文件路径
   document.head.insertBefore( myCSS, document.head.childNodes[         document.head.childNodes.length - 1 ].nextSibling ); // 插入到header的最后位置
   </script>
   ~~~

2. 将link元素的 `media` 属性设置为用户浏览器不匹配的媒体类型如：`media="print"`、`media="noexist"`

   - 对浏览器来说，如果样式表不适用于当前媒体类型，其优先级会被放低，会在不阻塞渲染的情况下再下载
   - 这么做只是为了实现CSS的异步加载，在文件加载完成之后，将`media`的值设为`screen`或`all`，从而让浏览器开始解析CSS

   ~~~HTML
   <link rel="stylesheet" href="mystyles.css" media="noexist" onload="this.media='all'">
   ~~~

3. 可以通过 `rel` 属性将 `link` 元素标记为 `alternate` 可选样式表，也能实现浏览器异步加载，同样别忘了加载完成之后，将 `rel` 改回去

   ~~~HTML
   <link rel="alternate stylesheet" href="mystyles.css" onload="this.rel='stylesheet'">
   ~~~

4. 使用 preload，比使用不匹配的 `media` 方法能够更早地开始加载CSS，所以尽管这一标准的支持度还不完善，仍建议优先使用该方法

   ~~~HTML
   <link rel="preload" href="mystyles.css" as="style" onload="this.rel='stylesheet'">
   ~~~

### 3.  CSS 文件压缩

文件的大小会直接影响浏览器的加载速度，往往效果显著，利用构建工具如 webpack对 CSS压缩 文件进行压缩，压缩后的文件能够明显减小，从而大大降低了浏览器的加载时间

### 4. 去除无用 CSS

一般情况下，会存在这两种无用的CSS代码：

- 一种是不同元素或者其他情况下的重复代码，在编写的代码时候，我们应该尽可能地提取公共类，减少重复
- 一种是整个页面内没有生效的 CSS 代码，如果手动删除这些无用CSS是很低效的，例如可以使用专门用于擦除未使用的 CSS 的 Webpack 插件—— `purgecss-webpack-plugin`

### 5. 有选择地使用选择器

1. 保持简单，不要使用嵌套过多过于复杂的选择器

2. 通配符和属性选择器效率最低，需要匹配的元素最多，尽量避免使用

3. 不要使用类选择器和ID选择器修饰元素标签，如：`h3#markdown-content`，这样多此一举，还会降低效率

4. 不要为了追求速度而放弃可读性与可维护性

> CSS选择器是从右向左匹配的

优点：能够使得 CSS 选择器在不匹配的时候效率更高

缺点：在匹配时匹配元素多的话，就要多耗费一些性能了

### 6. 减少回流重绘的次数

### 7. 尽量不要使用 @import

## CSS 常用属性

### 1. @media 响应式布局

- 直接写在 <style> 里面
- 通过 <link> 引入

[具体代码实现见]: D:/wdy-css/1、@media媒体查询用法

------

### 2. 元素的隐藏和显示

1. `display: none;` ：元素不可见，不再占据页面空间，无法响应点击事件，页面会发生回流重绘
2. `visibility: hidden;` ：元素不可见，占据页面空间，无法响应点击事件，页面只会发生重绘
3. `opacity: 0;` ：通过改变元素透明度来实现隐藏，占据页面空间，可以响应点击事件，页面只会发生重绘
4. `position: absolute; top: -9999px; left: -9999px;` ：通过将元素移出页面从而实现隐藏效果，不影响页面布局，无法响应点击事件
5. `width: 0; height: 0; margin: 0; padding: 0; border: 0; overflow: hidden;` ：通过将宽高等盒模型属性均设为`0`从而达到隐藏效果，不占据页面空间，无法响应点击事件
6. `clip-path: polygon(0px 0px,0px 0px,0px 0px,0px 0px);` ：通过裁减创建元素从而达到隐藏效果，占据页面空间，无法响应点击事件

> tips:

业务中常用的只有 `display: none`和 `visibility: hidden` 这2种方式，其他的方法只算隐藏页面元素的小技巧

------

### 3. background 属性

**background** 是一个 简写属性，一次性定义了所有的背景属性，包括 color, image, origin 还有 size, repeat 等

单独样式有以下：

| 值                    | 说明                                             | 默认值      | 版本   |
| --------------------- | ------------------------------------------------ | ----------- | ------ |
| background-color      | 指定要使用的背景颜色                             | transparent | CSS2.1 |
| background-position   | 指定背景图像的位置                               | 0%, 0%      | CSS2.1 |
| background-image      | 指定要使用的一个或多个背景图像                   | none        | CSS2.1 |
| background-repeat     | 指定如何重复背景图像                             | repeat      | CSS2.1 |
| background-attachment | 设置背景图像是否固定或者随着页面的其余部分滚动。 | scroll      | CSS2.1 |
| background-size       | 指定背景图片的大小                               | auto        | CSS3   |
| background-origin     | 指定背景图像的定位区域                           | padding-box | CSS3   |
| background-clip       | 指定背景图像的绘画区域                           | border-box  | CSS3   |

1. 在 CSS3 中，我们可以给元素添加多张背景图片

   - 背景图片所生效的样式，是属性值中与图片位置对应的值；
   - 如果属性值比背景图片的个数要少，那么没有对应的值的图片样式以第一个值为准；
   - 背景图片的层级按着从左往右，依次减小。当然，层级最低的还是 `background-color`；

   ~~~CSS
   .div1 {
       width: 100px;
       height: 100px;
   
       background-color: black;
       background-image: url('img1'), url('img2'); /* 两种图片:img1、img2 */
       background-size: 50%, 100%; /* img1 大小为50%，img2 大小为100% */ 
       background-repeat: repeat-x, no-repeat; /* img1 x轴上重复，img2 不重复 */ 
   }
   ~~~

2. 背景渐变 

   - 路径渐变：`background-image: linear-gradient();` （默认渐变方向：自下向上）

     具体用法详见： [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/gradient/linear-gradient) 

     ~~~CSS
     linear-gradient(to left top, orange 25%, yellow 50%, green 75%, blue 90%);
     ~~~

   - 重复路径渐变：`background-image: repeating-linear-gradient();` 

     ~~~CSS
     background-image: repeating-linear-gradient(45deg, #71c9ce 20px, #a6e3e9 30px, #e3fdfd 40px);
     ~~~

   - 径向渐变：`background-image: radial-gradient;` （默认渐变原点：中心点）

     具体用法详见： [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/gradient/radial-gradient) 

     ~~~CSS
     radial-gradient(circle at center, red 0, blue, green 100%);
     /* 简单居中渐变 */
     background-image: radial-gradient(cyan 0%, transparent 20%, salmon 40%);
     
     /* 非居中渐变 */
     background-image: radial-gradient(farthest-corner at 40px 40px, #f35 0%, #43e 100%);
     ~~~

   - 重复径向渐变：`background-image: repeating-radial-gradient();` 

     ~~~CSS
     background-image: repeating-radial-gradient(circle, #90ade4 ,#3350ba 20%);
     ~~~

3. 背景定位

   1. `background-position` 默认的定位为 `padding-box` 盒子的左上角，坐标为 (0, 0) 或 (left, top)

      即所设置元素的 `padding` 所占的区域，位于 `border`的内层、`content` 的外层

   2. 其属性值可设置为：百分比（%)、像素（px）、位置（`top` | `right` | `bottom` | `left` | `center`）

   3. 在只设置一个值的时候，另外一个值默认为 `center` 或 50% 

   ~~~CSS
   background-position: right; /* 右边居中位置 */
   ~~~

4. 背景重复

   `background-repeat` 除了常见的几个 repeat、repeat-x，repeat-y 以及 no-repeat 以外，还在CSS3 中新加了两个值： 

   - 背景图片小于容器时：
     - `space` ：在保证不缩放的前提下尽可能多的重复图片，并等分图片中间的空隙
     - `round` ：在尽可能多的重复图片的前提下，拉伸图片以铺满容器
   - 背景图片大于容器时：
     - `space` ：在不缩放的前提下裁剪图片，只保留在容器内的部分
     - `round` ：缩小图片以铺满容器，长宽与容器尺寸一致（未按比例缩放，图片极有可能变形）

5. 背景图片的相对位置

   `background-origin` 属性规定 `background-position` 属性相对于什么位置来定位，属性值有：

   -  `content-box` ：元素的 `padding` 所占区域包围着的即为 `content`
   - `padding-box` ：默认，即所设置元素的 `padding` 所占的区域，位于 `border`的内层、`content` 的外层
   - `border-box` ： 即所设置元素的 `border` 所占的区域，位于 `padding` 和 `content` 的外层

6. 整个背景的绘制区域

   `background-clip` ：属性规定背景的绘制区域，默认值 `border-box`，属性值同 `background-origin` 一样

7. 背景图片大小

   `background-size` 除了常见的设置大小（px）和百分比（%）之外，还有两个特殊的属性： 

   - `contain` ：当图片长宽不相同时，把图片按比例缩小至较长的一方完全适应内容区域为止，多用于背景图片比元素大的情况。

   -  `cover` ：当图片长宽不相同时，把图片按比例放大至较短的一方完全适应内容区域为止，以使背景图像完全覆盖背景区域，多用于背景图片比元素小的情况

8. 背景固定

   - `background-attachment: scroll` ：背景随页面滚动而滚动（默认）

   -  `background-attachment: fixed;` ：滚动页面的时候，背景图片是固定的

------

### 4. box-shadow 属性

用于在元素的框架上添加一个或多个阴影效果，每一个阴影效果的属性值有：

- `inset` ：如果没有指定 `inset`，默认阴影在边框外，即阴影向外扩散，  `inset` 会使得阴影落在盒子内部
- `offset-X ` ：水平偏移量，正值阴影则位于元素右边，负值阴影则位于元素左边
- `offset-y` ：垂直偏移量，正值阴影则位于元素下方，负值阴影则位于元素上方
- `blur-radius` ：模糊半径，值越大，模糊面积越大，阴影就越大越淡，不能为负值，默认为 0
- `spread-radius` ：扩散半径，取正值时，阴影扩大；取负值时，阴影收缩，默认为 0
- `color` ：颜色，如果没有指定，则由浏览器决定——通常是 `color` 的值，不过目前 Safari 取透明

除了常见的容器阴影效果外，还可以做出其他一些效果：

1. 立体效果：通过在元素相邻的边都添加阴影阴影效果
2. 画框效果：通过将阴影效果设置在元素里面
3. 锯齿效果：通过将同一个 X轴/ Y轴 上的一排多个阴影添加到元素上，从而产生视觉上的镂空锯齿效果
4. 点阵效果：把一个或多个阴影添加到元素上。可以设置X轴和Y轴的距离，形成不同的行和列，实现点阵效果
5. 彩蛋效果：通过将内阴影和外阴影结合，可以实现立体效果

------

### 5. border 和 outline

- `border` ：是设置元素多个边框属性的 简写属性
  - `border-style` ：边框样式
  - `border-width` ：边框宽度
  - `border-color` ：边框样式
- `outline`：是设置元素多个轮廓属性的 简写属性
  - `outline-style` ：轮廓样式
  - `outline-width` ：轮廓宽度
  - `outline-color` ：轮廓颜色

- 两者的区别：
  - outline 不占据空间，绘制于元素内容周围
  - 根据规范，outline 通常是矩形，但也可以是非矩形的
  - 将 outline 设置为 `0` 或 `none` 会移除浏览器的默认聚焦样式，比如去除 <input> 输入的默认聚焦样式

## CSS 水平垂直居中

### 行内元素

> 例如：文本text、图片<img>、按钮<button>、超链接<a>等行内元素

- 水平居中

  - 父节点声明： `text-align: center;`
  - 若父节点不是行内元素需声明： `display:inline | inline-block;`

  ~~~CSS
  .center{
    display:inline | inline-block;
    text-align:center;
  }
  
  <div class="center">文本text 水平居中</div>
  ~~~

- 垂直居中

  - 父节点设置 `line-height` 等于 `height`
  - 若节点不是行内元素需声明：`display:inline | inline-block;`

  ~~~CSS
  .center6{
    height: 100px; 
    line-height: 100px; 
  }
  
  <div class="center">文本text 垂直居中</div>
  ~~~

------

### 定宽和定高的块级元素

- 定宽块级元素水平居中
  1. position + 负margin + width
     - `position: absolute; left: 50%;`
     - `margin-left:  -width/2;`
  2. `margin: 0 auto;` + width
     - 给需要居中的块级元素添加：`margin: 0 auto;`
     - 这里块状元素的宽度 width 一定要有，若节点宽度已隐式声明则无需显式声明

- 定高块级元素垂直居中
  1. position + 负margin + height
     - `position: absloute; top: 50%;`
     - `margin-left:  -height/2;`
  2. `display: table;` + `display: table-cell;  vertical-align: middle;`
     - 父级 div 添加：`display: table;`
     - 给需要居中的块级元素添加：`display: table-cell;  vertical-align: middle;`

------

### 块级元素

- 水平居中
  1. margin: 0 auto + width: fit-content
  2. position父相子绝  + transform: translateX(-50%)
  3. display: flex + justify-content: center
- 垂直居中
  1. position父相子绝  + transform: translateY(-50%)
  2. display: flex + flex-direction: column + align-items: center | align-self: center
  3. display: flex + margin: auto 0
     - 父节点中声明 ：`display: flex;`
     - 子节点声明 ：`margin: auto 0;`

------

## CSS 技巧记录

### 1. 文本省略

- 单行省略 

  ~~~CSS
  .single-line-overflow {
      width: 200px; /*长度超过 200px 省略*/
      overflow: hidden; /*隐藏超出的文本内容*/
      text-overflow: ellipsis; /*用省略符号…来代表被修剪的文本*/
      white-space: nowrap; /*设置文字在一行显示，不能换行*/
  }
  ~~~

- 多行省略

  ~~~CSS
  .multi-line-overflow {
      width: 200px;
      overflow: hidden;
      position: relative;
      max-height: 90px;
      line-height: 30px;
  }
  .multi-line-overflow::after {
      position: absolute;
      right: 0;
      bottom: 0;
      /* background: linear-gradient(to right, transparent, #fff 50%); */
      background-color: #fff;
      content: "...";
  }
  ~~~

------

### 2. 



## CSS 图片处理

### 1. 二倍图

二倍图：我们准备的图片，比我们实际需要的大小 大2倍，这就就是 二倍图

> 示例：

将一个（50 x 50px） 的图片放到 Iphone6 里面就会放大2倍，变成（100 x 100px），图片就会模糊

处理方法：

我们采取的是放一个（100 x 100px）图片，然后手动的把这个图片的 宽高 设置为 （50 x 50px） 即可

补充：如果是背景二倍图，就是 将 background-size 设置为 50px 50px

------

### 2. CSS sprite (精灵图)

 1. 是什么
	把多个小图标合并成一张大图片
 2. 优缺点
  优点：减少了 http 请求的次数，提升了性能
  缺点：维护比较差（例如图片位置进行修改或者内容宽高修改）

> 实例：

1.  使用背景图引入图片： `background: url(图片路径) no-repeat;` 
2. 使用背景定位到显示需要的图标：`background-position: -23px -88px;`

------

### 3. 多张图片叠在一起

>  CSS 混合模式 mix-blend-mode ，相当于PS 中的图层叠放在一起(这个属性的兼容性不是很好)

~~~CSS
.item img{
  width: 150px;
  height: 200px;
}
.item img:first-child{
  position: absolute;
  mix-blend-mode: multiply;
}

<div class="item">
  <img src="./images/user_border.jpg" alt="">
  <img src="./images/user1.jpg" alt="">    
</div>
~~~

------

> position定位

~~~CSS

~~~

------

### 4. 



## CSS 动画记录

### 1. 消除父元素动画对子元素的影响

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

------

### 2. 