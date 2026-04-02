# flex
父：

|   |   |
|---|---|
|**属性**|**作用**|
|flex-direction|主轴方向（row / row-reverse / column / column-reverse）|
|justify-content|主轴对齐方式（start / center / space-between / space-around / space-evenly）|
|align-items|交叉轴对齐方式（stretch / center / flex-start / flex-end / baseline）|
|flex-wrap|是否换行（nowrap / wrap / wrap-reverse）|
|align-content|多行交叉轴对齐（只有多行才生效）|
子

|  |  |
| ---- | ---- |
| **属性** | **说明** |
| flex | 简写：flex-grow flex-shrink flex-basis |
| order | 排序 |
| align-self | 单个项目的交叉轴对齐 |
|  |  |
|  |  |
flex: 1   `flex-grow: 1`、`flex-shrink: 1` 和 `flex-basis: 0%` 的缩写。多元素设置该属性时，它们会按照比例平分剩余空间，常用于实现“左固定、右自适应”或“等分布局”。
# 两栏布局
* flex => width: 200px => flex: 1
* float: left  =>  width: 200px  =>  overflow: hidden
* float: left  =>  width: 200px =>  margin-left: 200px
* position: relative  =>  position:absolute  =>  width:200px  =>  left: 0  =>  margin-left: 200px
# 三栏布局
* flex   =>   width : 200px   =>   flex : 1
* width : 200px    =>    float : left    =>    float : right    =>    margin : 0 200px
* 相对绝对  =>  left : 0px  =>  right : 0 px  => left 200px+right 200px
* grid  => grid-template-columns: 200px auto 200px
# BFC
## 描述
独立渲染区域  =>  块级元素参与  =>  内部布局不影响外部元素，反之亦然
## 触发条件
- `float` `position`  `display` `overflow` 
## 特性/作用
margin合并， 高度塌陷， 文字环绕， 复杂布局
## 使用场景
清除浮动， 防止margin合并， 两栏布局
# 水平 & 垂直居中
###### 块级元素水平居中
* margin: 0 auto
###### 行内元素水平居中
* 父：text-align: center
###### 垂直居中(单行文字)
height = line-height 
###### 水平+垂直居中
* flex + justify-content : center + align-items : center
* 绝对定位 + top + left + tarnsform: translate往回拉
* 绝对定位 + top + left + margin-left +margin-top往回拉
###### 水平+垂直居中拓展方法
* grid + place-item: center
* table-cell + vertical-align: middle + text-align

# 响应式布局
百分比，媒体查询，flex, grid, rem/em, vw/vh

# display属性
none, inline, block, inline-block, flex, grid, table 
# position
static, relative, absolute, fixed, stricky(滑动到某个点变fixed)
脱标: absolute, fixed
# CSS新特性
box-shadow, border-radius, transition/animation, transform, flex/grid
# 伪类与伪元素区别
伪类：选择处于特定状态的元素，动态交互,结构选择， 作用于实际DOM元素，:hover, :focus, nth-child(), :first-child()
伪元素: 创建不在DOM树中的虚构内容， 添加内容,装饰性效果，作用于虚拟节点，
::before, ::after, ::selection
# 画三角形
w0 h0, border: 10px, solid, transparent, border-top-color: red

