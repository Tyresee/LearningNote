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

### 如何判断页面的滚动方向
### 如何在vue中完成上拉加载下一页（分页）
