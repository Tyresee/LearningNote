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
var sayHi = function(name,message){
    console.log(`Hello${name}${message}`)
}
```
```
var sayHi = function(){
    console.log(`Hello${arguments[0]}${arguments[1]}`)
}
```

用箭头函数的话，写法有区别，因为箭头函数不绑定arguments，取而代之用...操作符解决

```
let sayHi = (name,message)=>{
    console.log(`Hello${name}${message}`)
}
```
```
let sayHi = (...arguments)=>{
    console.log(`Hello${arguments[0]}${arguments[1]}`)
}
```
//此处的arguments不是非箭头函数中的关键词，只是形参，可以替换成别的,可以写成下面这样
```
let sayHi = (...arg)=>{
    console.log(`Hello${arg[0]}${arg[1]}`)
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

### Array.prototype.join()
```
let a = [1,2,3,4]
a.join('||')  // =>"1||2||3||4"
a.join(undefined)  // =>"1,2,3,4"

```
### grid布局
CSS
```
.wrapper {
  display: grid;
  grid-template-columns: 30% 20% 40% 10%;
  /* grid-template-rows: 80px 70px; */
}
.item {
  text-align: center;
  color: blueviolet;
  font-size: 30px;
  margin:0 10px 10px 0;
  display: flex;
  align-items: center;
  justify-content: center;

}
.item>div{
  word-wrap: break-word;
  word-break:break-all;
}
.item:nth-child(odd) {
  background-color: rgba(44, 167, 249, 0.7);
}
.item:nth-child(even) {
  background-color: rgba(44, 167, 249, 0.9);
}
```
HTML
```
<div class="wrapper">
  <div class="item item1"><div>1</div></div>
  <div class="item item2"><div>1</div></div>
  <div class="item item3"><div>1</div></div>
  <div class="item item4"><div>111111111111</div></div>
  <div class="item item5"><div>1</div></div>
  <div class="item item6"><div>1</div></div>
  <div class="item item7"><div>1</div></div>
</div>
```

### this和箭头函数
```
window.n = 'window name'
var obj = {
    n: 'obj name',
    say: function(){
        console.log(this.n)
    }
}

obj.say() //=>'obj name'
```

```
window.n = 'window name'
var obj = {
    n: 'obj name',
    say:()=>{
        console.log(this.n)
    }
}

obj.say() //=>'window name'
```
```
window.n = 'window name'
let obj = {
    n: 'obj name',
    sayN(){
        console.log(this.n)
    }
}

let fn = obj.sayN
fn() //=>'window name'
```
在对象中分别用function关键词和箭头函数来作为say的值,对应的this是不同的。
箭头函数的this看外层的是否有函数，如果有，外层函数的this就是内部箭头函数的this，如果没有，则this是window


### 碰到小程序上面关于filter: grayscale(1)导致的问题
在一个scroll-view里某个部分使用这个属性，在滚动时，出现类似"迟滞"、“残影”的bug。这个问题只在真机上出现（多个机型）,在模拟器上没有重现,在页面底部的"确认下单"按钮（固定位置的元素）也没有出现这种问题。所以推断是在scroll-view中滚动元素的bug。
![Global对象的其他属性](https://upload-images.jianshu.io/upload_images/7557569-b88fac92fe3c2b68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)
解决办法：用灰色图片来取代filter: grayscale(1)的方案。

### ES6的decorator
> 修饰器是一个对类进行处理的函数。修饰器函数的第一个参数，就是所要修饰的目标类</br>
> 修饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。这意味着，修饰器能在编译阶段运行代码。也就是说，修饰器本质就是编译时执行的函数

### DOM事件级别
DOM0中关于事件的标准 element.onclick = ()=>{}
DOM1中没有制定事件相关的标准
DOM2中关于事件的标准 element.addEventListener('click',()=>{},false) //false 代表“捕获”跟"冒泡"
DOM3中关于事件的标准 element.addEventListener('keyup',()=>{},false) //false 代表“捕获”跟"冒泡",在制定DOM3的时候，在DOM2中事件相关的标准的基础上增加了一些事件监听

### DOM事件模型
捕获(从不具体到具体) 冒泡（从具体到不具体）

### DOM事件流(简单)
三个阶段：1捕获 2目标阶段(从捕获到达目标元素的阶段) 3冒泡（从目标对象再上传到window对象）

### DOM事件捕获（从不具体到具体）的具体流程
第一个接受到事件的对象：window
过程：window->document->html->body->元素一层一层往下一直到目标元素
冒泡对象跟上述顺序相反
HTML
```
<div id="wrapper">
  <button id="btn"></button>
</div>
```
```
var wrapper = document.getElementById("wrapper")
var btn = document.getElementById("btn")

btn.addEventListener("click", function (e) {
  console.log('doing bubbling one')
}, false)

wrapper.addEventListener("click", function (e) {
  console.log('doing capture')
  e.stopPropagation() //会在这里停止执行，其他几个函数都不会执行,包括“doing bubbling one”也不会打印
}, true)

btn.addEventListener("click", function (e) {
  console.log('doing bubbling two')
}, false)

btn.addEventListener("click", function (e) {
  console.log('doing bubbling three')
}, false)
```


### event对象的常见应用
1.event.preventDefault() 阻止默认事件，比如阻止a标签默认的跳转行为
2.event.stopPropagation() 阻止冒泡
3.event.stopImmediatePropagation() 如果有多个相同类型事件的事件监听函数绑定到同一个元素，当该类型的事件触发时，它们会按照被添加的顺序执行。如果其中某个监听函数执行了 event.stopImmediatePropagation() 方法，则当前元素剩下的监听函数将不会被执行
```
var btn = document.getElementById("myBtn")
btn.addEventListener("click", (e)=>{ 
  alert('doing one')
}, false)
btn.addEventListener("click", (e)=>{
  alert('doing two')
  e.stopImmediatePropagation()//后续的监听函数不会执行，但是本函数下的代码会执行完
  alert('doing two..') //这里会执行
}, false) //这几个事件处理函数回执行到此处
btn.addEventListener("click", (e)=>{
  alert('doing three')
}, false)

```

event.target 当前被点击的元素  
event.currentTarget 当前所绑定的事件 

> 通过 addEventListener()添加的事件处理程序只能使用 removeEventListener()来移除；移 除时传入的参数与添加处理程序时使用的参数相同
### 自定义事件
let eve = new Event('custome')
el.addEventListener()

### Array.from 可以将“类似数组的对象”、set、map等数据结构变成数组
所谓“类似数组的对象”本质上就是有length属性的对象,也就是说，某个对象有length属性，那么它就是“类似数组的对象”

### 快速地生成一个长度为10的数组
```
let a = Array.from({length:10}).map((e,i)=>i) //=>[0,1,2,3,4,5,6,7,8,9]
```
### parseInt(string,radix)
string:必选参数。要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用  ToString 抽象操作)
radix:可选参数。一个介于2和36之间的整数(数学系统的基础)，表示上述字符串的基数。比如参数"10"表示使用我们通常使用的十进制数值系统。始终指定此参数可以消除阅读该代码时的困惑并且保证转换结果可预测。当未指定基数时，不同的实现会产生不同的结果，通常将值默认为10。

parseInt('123', 5) // 将'123'看作5进制数，返回十进制数38 => 1*5^2 + 2*5^1 + 3*5^0 = 38

### webpack配置后，执行yarn build碰到了问题
![webpack配置后，执行yarn build碰到了问题](https://i.loli.net/2019/02/28/5c77e24b60587.png)

google搜索“Plugin/Preset files are not allowed to export objects, only functions”这个报错,找到https://github.com/babel/babel/issues/8707，在下面提到了.babelrc的写法，将其修正为与webpack.common.js中对应的设置相同，即可成功build。

### 小程序的view组件有几个事件句柄没有在文档中体现出来
bindtouchend bindtouchstart bindtouchmove

### justify-content: end 没有生效？
因为正确写法是：justify-content: flex-end

### 在create-react-app 创建的项目中引入scss
yarn add sass-loader node-sass -D

### css中的 @import 末尾的分号不能省略
```
@import '~antd/dist/antd.css';  //√
@import '~antd/dist/antd.css'   //X
```

### 执行 npm run eject 时，报错 Remove untracked files, stash or commit any changes, and try again
```
git init 
git add .
git commit -am 'commit before ejecting'
```

### react 用户定义组件必须以大写字母开头,否则报错：TypeError: Super expression must either be null or a function
```
class something extends React.component{
  
}   // X

class Something extends React.Component{
  
}   // √

```

### 一个箭头函数的细节
```
let a = param => {name:'whatever'}  //X

```
这是因为花括号 {} 里面的代码被解析为一系列语句,而非对象字面量的组成部分
所以，记得用圆括号把对象字面量包起来：
```
let a = param => ( {name:'whatever'} ) //√
```

### 微法院项目图片太多导致编译报错
![微法院项目图片太多导致yarn build /yarn dev报错](https://i.loli.net/2019/04/11/5caea1ac4e107.png)

google ENFILE: file table overflow
第一个结果：https://github.com/meteor/meteor/issues/8057

### 使用 console.log() 来打印某个对象，并且，两次打印之间，还会对这个对象进行修改，最后我们查看打印的结果发现，修改前的打印和修改后的打印，是一样的
解决方法：
> 打印一个从这个对象复制出来的对象。
> 使用资源面中的断点来调试
> 使用 JSON.stringify() 方法处理打印的结果

### yarn dev 一个vue项目，报错：{ parser: "babylon" } is deprecated; we now treat it as { parser: "babel" }
解决办法：
找到你的工程文件夹里的 YourProName\node_modules\vue-loader\lib\template-compiler\index.js
```
//将以下代码
if (!isProduction) {
  code = prettier.format(code, { semi: false, parser: 'babylon' })
}
 
//修改为：
 
if (!isProduction) {
  code = prettier.format(code, { semi: false, parser: 'babel' })
}

```

### 短路运算
我们可以使用 '与' && 和 '或' || 逻辑运算符以更简洁的方式书写表达式。 这通常被称为“短路”或“短路运算”。

|| 寻找并返回第一个“真”值；&& 寻找并返回第一个“假”值；如果它们连接的每一个操作数都不是目标值，就返回最后一个计算过的表达式。(“或”真“与”假)
```
假1 || 假2 || 真1 || 假3 || 真2 || 真3  // => 真1
真1 && 真2 && 假1 && 假2 && 真3 && 假3  // => 假1
```

### webstorm打开项目找不到src
解决办法：找到.idea文件夹，删掉

### axios 使用post方式传递参数，后端接受不到
解决办法：https://segmentfault.com/a/1190000012635783?utm_source=tag-newest
引入qs

### 引入mint-ui中的cell布局组件，@click触发不了事件函数
解决办法：用@click.native;(在 Vue 2.0 中，为自定义组件绑定原生事件必须使用 .native 修饰符：
<my-component @click.native="handleClick">Click Me 官方文档有说)

### ArrayBuffer\TypedArray\DataView
ArrayBuffer对象：代表内存之中的一段二进制数据，可以通过“视图”进行操作。“视图”部署了数组接口，这意味着，可以用数组的方法操作内存。
TypedArray视图：共包括 9 种类型的视图，比如Uint8Array（无符号 8 位整数）数组视图, Int16Array（16 位整数）数组视图, Float32Array（32 位浮点数）数组视图等等。
DataView视图：可以自定义复合格式的视图，比如第一个字节是 Uint8（无符号 8 位整数）、第二、三个字节是 Int16（16 位整数）、第四个字节开始是 Float32（32 位浮点数）等等，此外还可以自定义字节序。
简单说，ArrayBuffer对象代表原始的二进制数据，TypedArray视图用来读写简单类型的二进制数据，DataView视图用来读写复杂类型的二进制数据。

### 上传图片(ArrayBuffer的应用)
上传图片需要处理图片的宽高分辨率等等，图片本身是fileObj,需要转成base64,再转canvas,再转ArrayBuffer,再转Blob，再转formData,然后上传.

### FormData
现代 Web 应用中频繁使用的一项功能就是表单数据的序列化，XMLHttpRequest 2级为此定义了 FormData 类型。FormData 为序列化表单以及创建与表单格式相同的数据（用于通过 XHR 传输）提供 了便利。

### Service Worker 
原生App拥有Web应用通常所不具备的富离线体验，定时的静默更新，消息通知推送等功能。而新的Service workers标准让在Web App上拥有这些功能成为可能。
一个 service worker 是一段运行在浏览器后台进程里的脚本，它独立于当前页面，提供了那些不需要与web页面交互的功能在网页背后悄悄执行的能力。在将来，基于它可以实现消息推送，静默更新以及地理围栏等服务，但是目前它首先要具备的功能是拦截和处理网络请求，包括可编程的响应缓存管理。
为什么说这个API是一个非常棒的API呢？因为它使得开发者可以支持非常好的离线体验，它给予开发者完全控制离线数据的能力。

### 想用a分支上的内容覆盖b分支上的内容
```
git checkout b <br>
git reset --hard origin/master <br>
git push -f
```

### list的最后一项没有border-bottom

CSS
```
<ul>
  <li> 11111</li>
  <li> 11111</li>
  <li> 11111</li>
  <li> 11111</li>
  <li> 11111</li>
  <li> 11111</li>
</ul>
```
HTML
```
ul {
  list-style: none;
  padding:0;
  margin:0;
  border:1px solid red;
  width:400px;
}

ul>li+li {
  border-top:1px solid red;
}
```

### vue项目里引入pdf.js实现pdf的预览功能
1.在这个链接下载pdf.js整个包http://mozilla.github.io/pdf.js/getting_started/#download  作为一个独立项目部署到服务器上(获取访问地址)
2.创建pdfViewer组件，并在路由中注册
pdfViewer/index.vue
```
<template>
  <div>
    <iframe :src="pdfUrl" class="pdf-window" width="100%" style="height: 100vh;">
    </iframe>
  </div>
</template>

<script>
  const pdfPrefix = 'https://testwecourt.odrcloud.cn/ghodr-zwwx/pdfjs/web/viewer.html'//pdf.js部署到服务器可访问的url
  export default {
    name: 'pdfViewer',
    data() {
      return {
        pdfUrl: '',
      };
    },
    mounted() {
      this.pdfUrl = `${pdfPrefix}?file=${encodeURIComponent(this.$route.params.fileUrl)}`;
    },
  };
</script>

```

route/routes.js
```
export const routes = [
  {
    path: '/',
    component: () => import('@/views/Home.vue'),
  },
  //.......
  {
    path: '/pdfViewer',
    component: () => import('@/views/pdfViewer/index.vue'),
    name: 'pdfViewer',
  },
];

```
其他页面跳转到pdfViewer带上pdf的url,需要后台允许跨域(参考https://github.com/goSunadeod/vue-pdf.js-demo)
```
previewFile({
  documentId: fileId,
  caseId: this.caseId,
}).then((res) => {
  const fileUrl = res.data;
  this.$router.push({ name: 'pdfViewer', params: { fileUrl } });
});
```
### 小程序选择dom元素，是一个异步api

```
wx.createSelectorQuery()
.select('#slider')
.boundingClientRect(res => {
  sliderWidth = res.width
}).exec()


```

### wepy 1.X 循环渲染组件碰到的坑

#### 用wx:for 循环出来的几个组件，状态都是最后一个组件的状态
解决办法: 用repeat组件来做循环

父组件template
```
<repeat for="{{keyValuePairList}}" index="index" item="item" key="index">
  <KeyValuePair
    :keyName.sync="item.keyName"
    :placeholder.sync="item.placeholder"
    :isLink="item.isLink"
    @setValue.user="setValue"/>
</repeat>
```
子组件script
```
export default class KeyValuePair extends wepy.component {
    props = {
      keyName: {
        type: String,
        default: '姓名',
        twoWay: true
      },
      placeholder: {
        type: String,
        default: '请输入',
        twoWay: true
      },
      isLink: {
        type: [Boolean, String],
        coerce(v) {
          return typeof v === 'string' ? JSON.parse(v) : v //这么写来解决父组件传Boolean时，子组件收不到的问题
        },
        default: false,
      }
    }
    methods = {
      handleInput: (e) => {
        this.$emit('setValue', e.detail.value, this.keyName)  
      }
    }
  }
```

发现了另一个问题，子组件打印this.keyName是undefined。(页面展示是对的)
解决办法: 
> WePY 1.x 版本中，组件使用的是静态编译组件，即组件是在编译阶段编译进页面的，每个组件都是唯一的一个实例，目前只提供简单的 repeat 支持。不支持在 repeat 的组件中去使用 props, computed, watch 等等特性。
所以只能对于需要在方法中获取prop值的情况，只能在父组件的component里声明不同的单独组件
```
components = {
  KeyValuePair_0: KeyValuePair,
  KeyValuePair_1: KeyValuePair,
  KeyValuePair_2: KeyValuePair,
}
```

### 父元素overflow-x: scroll；padding: 0 35rpx; 发现右边的padding没有生效(最右边的子元素是贴着父元素最右边)

解决办法：给最后一个元素加个伪元素，撑开右边
HTML
```
<view class="options-wrapper">
  <view class="option" wx:for="{{optionList}}">{{item.name}}</view>
</view>
```
CSS
```
.options-wrapper{
 width: 750rpx;
 overflow-x: scroll;
 box-sizing: border-box;
 padding:0 37rpx;
 display: flex;

 >.option{
   height: 56rpx;
   line-height: 56rpx;
   flex-shrink: 0;
   text-align: center;
   padding: 0 24rpx;
   border-radius: 5rpx;
   border: solid 1rpx #4f8ff3;
   font-size: 28rpx;
   color: #4f8ff3;
   margin-right: 30rpx;
   margin-top: 37rpx;

   &:last-child{
    position: relative;
     &::after{
       content: '';
       position: absolute;
       height: 1rpx;
       width: 37rpx;
       right: -37rpx;
       opacity: 0;
     }
   }
  }
}
```

### 父元素display: flex; 子元素左对齐(除了最右边的子元素)，最右边的子元素右对齐
解决办法: 给最右边的子元素加上margin-left: auto
参考文档： 
</br>
[最右边子元素右对齐，其他子元素左对齐](https://stackoverflow.com/questions/49658425/flexbox-justify-self-flex-end-not-working?noredirect=1&lq=1)  
</br>
[最左边子元素左对齐，其他子元素右对齐](https://stackoverflow.com/questions/23621650/how-to-justify-a-single-flexbox-item-override-justify-content)

### test