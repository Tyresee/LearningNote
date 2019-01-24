## 前端零散知识点

### 小程序获取某个节点
```
wx.createSelectorQuery().select('#some_id')
```

### boundingClientRect
> 添加节点的布局位置的查询请求。相对于显示区域，以像素为单位。其功能类似于 DOM 的 getBoundingClientRect</br>
> 使用场景：想获取某个DOM元素的位置（相对于视窗的left、top还有宽高等等信息）
### 构造函数
> 没有形参的构造函数调用都可以省略圆括号</br>
> 下面两种写法是等价的
```
let o = new Object();
let o = new Object;
```
### call和apply的意义
> 这两个方法都允许显式地指定调用所用的this值，也就是说，***任何函数*** 可以作为 ***任何对象的方法*** 来调用

### 形参 实参
> 形参就是占位的，实参是调用函数时候真正传入的值</br>
> 函数的实参可选时，往往传入一个无意义的占位符，通常用null占位，也可以用undefined占位

### 柯里化
> 柯里化是把*接受多个参数的函数*变换成接受一个*单一参数*
> （最初函数的第一个参数）的函数，并且返回接受余下的    参数而且返回结果的新函数的技术</br>
#### 柯里化前
```
 function add(a, b) {
     return a + b;
 } 
 // 执行 add 函数，一次传入两个参数即可
 add(10, 2) // 12

```
#### 柯里化后
```
 var add = function(a) {
    return function(b) {
        return a + b;
    };
 };
 var addTen = add(10);
 addTen(2); // 12
```
#### 那么我们为什么要使用柯里化呢？

> 1、参数复用</br>
 2、延迟计算 </br>
 3、提前返回

首先柯里化可以使得参数复用并且延迟计算，也就是说像上面的例子，比如我们的 a 值一直都是 10，只是 b 值在变化，那么这个时候用到柯里化，就可以减少一些重复性的传参。</br>比如 b 是 2，3，4,那么在柯里化前的调用就是：
```
add(10,2)
add(10,3)
add(10,4)
```
而在柯里化之后，就像上面的例子中写的，我们通过定义
```
var addTen = add(10);
```
将第一个参数 a 预置了 10，之后我们只需要调用
```
addTen(2)
addTen(3)
addTen(4)
```
而在这个过程中，如果使用柯里化前的代码，或当即就把结果计算出来，而在柯里化之后，我们可以现传入一个 10，然后在想得到真正结果的时候再传入另一个参数，体现了*延迟计算*</br>
同时柯里化函数还可以 *提前返回*，很常见的一个例子，兼容现代浏览器以及IE浏览器的事件添加方法。我们正常情况不使用柯里化可能会这样写:</br>
```
var addEvent = function(el, type, fn,capture) {
    if (window.addEventListener) {
        el.addEventListener(type, function(e) {
            fn.call(el, e);
        }, capture);
     } else if (window.attachEvent) {
        el.attachEvent("on" + type, function(e) {
            fn.call(el, e);
        });
    } 
 };
```
这个时候我们没调用一次 addEvent，就会进行一次 if else 的判断，而其实具体用哪个方法进行方法的绑定的判断执行一次就已经知道了，所以我们可以使用柯里化来解决这个问题：
```
var addEvent = (function(){
    if (window.addEventListener) {
          return function(el, sType, fn, capture) {
            el.addEventListener(sType, function(e) {
                fn.call(el, e);
            }, (capture));
        };
    } else if (window.attachEvent) {
        return function(el, sType, fn, capture) {
            el.attachEvent("on" + sType, function(e) {
                fn.call(el, e);
            });
        };
    }
})();
```


### 栈(stack)
先进后出，像个有底部的箱子(而不是没有底部的管道)

### QQ排查DNS问题

可是为什么 DNS 不对，QQ 却可以正常上呢？ 后来我才知道， 因为 QQ 是直接使用 IP 地址来连接服务器的， 所以即便 DNS 失效， 它依然可以“屹立不倒”。

### 网络 数据包
包相当于*信件*或者*包裹*，而交换机和路由器则相当于邮局或*快递公司的分拣处理区*。包的头部存有目的地等控制信息，通过许多交换机和路由器的接力，就可以根据控制信息对这些包进行分拣，然后将 它们一步一步地搬运到目的地。

### 协议栈（网络 控制软件叫作协议栈）
从浏览器接收到的*消息打包*，然后加上目的地址等*控制信息*。如果拿邮局来比喻，就是把信装进信封，然后在信封上写上收信人的地址。</br>
例如当发生通信 错误时重新发送包，或者调节数据发送的速率等，或许我们可以把它当作 一位帮我们寄信的小秘书

### 网卡(负责以太网或无线网络通信的硬件)
会将包转换为电信号并通过网线发送出去

### 数组sort方法
```
let arr = [2,1,4,34,20]
arr = arr.sort((a,b)=>a-b) //[1,2,4,20,34]
```

### throw try catch
throw语句用来抛出一个用户自定义的异常。当前函数的执行将被停止（throw之后的语句将不会执行），并且控制将被传递到调用堆栈中的第一个catch块。如果调用者函数中没有catch块，程序将会终止。
```
function getRectArea(width, height) {
  if (isNaN(width) || isNaN(height)) {
    throw "Parameter is not a number!";
  }
}

try {
  getRectArea(3, 'A');
}
catch(e) {
  console.log(e);
  // expected output: "Parameter is not a number!"
}

```

### switch case (switch后面的值需要和case的值严格等)
合并两种情况的写法
```
switch (NUM) {
  case 1:
  case 2:
    console.log('case1 and case2');
    break;
  default:
    console.log('other case');
    break;
}
```
### arguments 
以下两种函数的声明是等价的
```
sayHi:(name,message) => {
    console.log(`Hello${name}${message}`)
}
```
```
sayHi:()=>{
    console.log(`Hello${arguments[0]}${arguments[1]}`)
}
```
### 贝塞尔函数&动画
transition 过渡属性后面的参数有四个（属性名，持续时间、规定速度效果的速度曲线、过渡效果的开始时间）

针对速度效果的速度曲线展开讨论，有如下几种情况：
linear--匀速
ease--慢->快->慢
ease-in--慢->匀
ease-out--匀->慢
ease-in-out--慢->快->慢
cubic-bezier(n,n,n,n) 自定义速度函数
可参考http://cubic-bezier.com/
<!-- transition: top .4s cubic-bezier(0.39,-0.4,0.83,0.23), left .4s linear; -->
HTML
```
<div class="father">
  <div class="son"></div>
</div>
```
SCSS
```
.father{
  width: 300px;
  height: 300px;
  background: pink;
  position:relative;
  transition:all 1s;
  .son{
    position:absolute;
    left:150px;
    bottom:150px;
    width: 10px;
    height: 10px;
    border-radius:5px;
    background: grey;
    transition:bottom 1s cubic-bezier(.23,-0.51,.71,-0.44),left 1s linear;
    
  }
}
```
JS
```
let son = document.getElementsByClassName('son')
son[0].style.left = '300px'       
son[0].style.bottom = '0' 
```

### IP地址
子网可以理解 为用集线器连接起来的几台计算机 ，我们将它看作一个单位，称为子网。 将子网通过路由器连接起来，就形成了一个网络。
在网络中，所有的设备都会被分配一个地址。这个地址就相当于现实 中某条路上的“×× 号 ×× 室”。其中“号”对应的号码是分配给整个子 网的，而“室”对应的号码是分配给子网中的计算机的，这就是网络中的 地址。“号”对应的号码称为网络号，“室”对应的号码称为主机号，这个 地址的整体称为 IP 地址

![IP的基本思路](https://upload-images.jianshu.io/upload_images/7557569-9d69a22dda78f957.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### wepy中点击组件元素，触发父容器的方法
组件：
HTML:
```
<template>
  <view class="compoWrapper" > 
    <text class="txt"  @tap.stop="execFunc">点击触发组件事件execFunc</text>
   </view> 
</template>
```
JS
```
<script>
  import wepy from 'wepy'
  export default class compo extends wepy.component {
    data = {
    }
    methods = {
      execFunc: ()=> {
        this.$emit('childFn', 100)
      }
    }
    props = {
    }
  }
</script>
```

页面：
HTML:
```
<template>
    <LongTapPopupMenu class="longTapPopupMenu" @childFn.user="pageFunc"></LongTapPopupMenu>
</template>
```
JS
```
<script>
  import wepy from 'wepy'
  export default class pageInstance extends wepy.page {
    data = {
    }
    methods = {
      pageFunc: ()=> {
        console.log('doing page thing');
      }
    }
    props = {
    }
  }
</script>
```
### 伪元素就像额外的免费 DOM 元素。 它们允许我们添加额外的内容、修饰和等等到我们的页面而不添加额外的 html 代码

### 普遍认为我们使用双冒号:: 来表示伪元素 (而不是像伪类: hover，:first-child)。 如果您要添加 IE8 的支持，最好使用: 来代替。 其他所有的浏览器和较新版本的 IE 支持双冒号

### 在遮罩层出现后，禁止页面的滚动事件。目前碰到的有效的解决办法
```
<template>
  <view class="wrapper {{stopScroll?'stop-scroll':''}}" >
    <view class="modal">
    <view/>
  <view/>
<template/>
```
```
<script>
  export default class IndexPage extends wepy.page {
    data = {
      modalShow:false
    }
    computed = {   
      stopScroll(){
        return this.modalShow
      }
    }
  }
<script/>
```
```
.stop-scroll{
  height: 100vh;//100%的视窗高度,（1vh就是视窗高度的1%）
  overflow: hidden;
}
```
### vue项目中进行rem的适配
index.html的<header>中加上
```
  <script>
    function rem() {
      var html = window.document.documentElement
      var width = Math.min(html.getBoundingClientRect().width, 500)
      html.style.fontSize = width * 100 / 750 + 'px'
    }
    rem()
    window.onresize = function () { rem() }
  </script>
```
在其他的组件和页面中可以用rem来写了。比如小程序中120rpx可以写成1.2rem；200rpx可以携程2rem。



### vue项目中修改config相关的东西时，要重新yarn dev编译！！

### 如何在vue中完成上拉加载下一页（分页）
### 禁止H5页面缩放，就在index.html中的header加上
```
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0;" name="viewport" />
```
### 测试H5
yarn build 完了以后，把dist文件夹发给后台部署

### oss上的图片加载失败的话，需要做默认占位图
html:  
```
<img class="avatar" :src="item.image" :onerror="defaultImg" />

```
"script"标签里

```
   let defaultImg = require('@/assets/img/defaultAvatar.png')
   ...
     data() {
       return {
         defaultImg: 'this.src="' + require('@/assets/img/defaultAvatar.png') + '"'
       }
     }

```
### 双方括号指的是“属性”的“特性”
只有内部才用的特性（attribute）时，描述了属性（property）的各种特征。 ECMA-262 定义这些特性是为了实现 JavaScript 引擎用的，因此在 JavaScript 中不能直接访问它们。为了 表示特性是内部值，该规范把它们放在了两对儿方括号中，例如[[Enumerable]]

### 研究"throw new Error('一个错误')" 跟 "console.error('一个错误')然后return终止运行函数的区别"
初步结论：console是*宿主对象*（也就是游览器提供的内置对象）。 用于访问调试控制台, 在不同的浏览器里效果可能不同。

### Global对象
> 《js高级程序设计》 Global（全局）对象可以说是 ECMAScript 中最特别的一个对象了，因为不管你从什么角度上看， 这个对象都是不存在的。ECMAScript 中的 Global 对象在某种意义上是作为一个终极的“兜底儿对象” 来定义的。换句话说，不属于任何其他对象的属性和方法，最终都是它的属性和方法。事实上，没有全 局变量或全局函数；所有在全局作用域中定义的属性和函数，都是 Global 对象的属性。本书前面介绍 过的那些函数，诸如 isNaN()、isFinite()、parseInt()以及 parseFloat()，实际上全都是 Global 对象的方法。Global对象还包括 encodeURI() 、encodeURIComponent() 以及对应的解码方法、eval()、window对象

</br>

![Global对象的其他属性](https://upload-images.jianshu.io/upload_images/7557569-e9e0479e3d53dc61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)

### 埋点
“埋点”就是在页面中的某些元素中加入一些‘触发’，可以通过这种方式，页面可以监听并统计用户行为。

### 如何自己实现一个typeof
曾经碰到的一个面试题。没有碰到需要自己实现类型检测的场景。这个问题在《js高级程序设计》中找到了答案。
> 5.2.1 检测数组</br>
> 自从 ECMAScript 3 做出规定以后，就出现了确定某个对象是不是数组的经典问题。对于一个网页， 或者一个全局作用域而言，使用 instanceof 操作符就能得到满意的结果：
> ```
> if (value instanceof Array){ //对数组执行某些操作 }
> ```
在ECMAScript5里新增了针对数组的类型检测函数isArray,支持 Array.isArray()方法的浏览器有 IE9+、Firefox 4+、Safari 5+、Opera 10.5+和 Chrome。要在尚未实现这个方法中的浏览器中准确检测数组，需要向下面这样操作(《js高级程序设计》第22章的22.1.1节)：
```
function isArray(value){
   return Object.prototype.toString.call(value) == "[object Array]"
 }
```
为什么要这么做呢？
> **JavaScript 内置的类型检测机制并非完全可靠。**
> 事实上，发生错误否定及错误肯定的情况也不在少 数。比如说 typeof 操作符吧，*由于它有一些无法预知的行为，经常会导致检测数据类型时得到不靠谱 的结果*。Safari（直至第 4 版）在对正则表达式应用 typeof 操作符时会返回"function"，因此很难确 定某个值到底是不是函数。
> 
 
用这个思路同样可以进行别的数据类型的检测
```
function isArray(value){
 return Object.prototype.toString.call(value) == "[object Array]"; }

 function isFunction(value){
 return Object.prototype.toString.call(value) == "[object Function]"; }

function isRegExp(value){
   return Object.prototype.toString.call(value) == "[object RegExp]"; }
```

### return 只能用在函数中，不能脱离函数使用
在控制台中执行如下代码是会报错的
```
return 1
```
怎么修正呢?答：改成立即执行函数
```
(()=>{ return 1 })()

```

### 好用的电子书网站http://www.booklist.mobi/

### 过滤掉“非值”
```
let a = [1,2,false,'false','',undefined,null,{}]
a.filter(Boolean)  //=>[1,2,'false',{}]
```
### object.hasOwnProperty()用来判断某个属性是来自原型链还是自身的属性

```
let o = new Object
o.prop = 'something'
o.hasOwnProperty('prop') //=>true  这是自身的属性
o.hasOwnProperty('toString') //=>false  toString是原型链上继承的方法
```

### wepy中@tap的函数中传参后，在方法中接收并打印，发现没有接收到，只是打印了事件对象e
后来发现是因为此代码块中的 idx 并没有用wx:for-index="idx"声明

### 深拷贝
```
let obj = {a:1}
let shallowCopyObj = a //浅拷贝，shallowCopyObj的指针指向obj
let deepCopyObj = JSON.parse(JSON.stringify(a)) //深拷贝，deepCopyObj的指针并不指向obj

```

### 给vscode设置系统中没有的字体
先在网上下载相应的tff文件,双击即可安装到系统
然后在vscode中点击command+','进入设置，搜索fontFamily,然后在右边用新的键值对覆盖默认设置。

默认设置：
```
"editor.fontFamily": "Menlo, Monaco, 'Courier New', monospace",
```
用如下设置覆盖：
默认设置：
```
"editor.fontFamily": "'Roboto Mono',Menlo, Monaco, 'Courier New', monospace",
```
