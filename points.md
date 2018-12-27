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