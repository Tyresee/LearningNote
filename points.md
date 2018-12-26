## 前端零散知识点

### 小程序获取某个节点
```
wx.createSelectorQuery().select('#some_id')
```

### boundingClientRect
> 添加节点的布局位置的查询请求。相对于显示区域，以像素为单位。其功能类似于 DOM 的 getBoundingClientRect</br>
> 使用场景：想获取某个DOM元素的位置（相对于视窗的left、top还有宽高等等信息）
