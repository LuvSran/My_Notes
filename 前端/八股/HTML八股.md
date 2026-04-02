# src 与 href 的区别
1. href 是超文本引用，指向目标资源地址，建立与目标资源之间的连接
2. src 是将资源下载到页面
3. href 不会阻塞页面的渲染，src 会阻塞页面的渲染
# 导入样式时，link 和 @import有什么区别
1. link 是 html 提供的标签，除了导入样式还支持导入 rel 连接属性,网站图标等，且只能存在于 head 内部。@import 是 css 提供的语法，只能用作导入样式
2. link 导入的样式是同时被加载，@import 则是页面加载完后才被加载
3. link 没有兼容性问题，@import 可能有兼容性问题，IE5 以上才支持
4. link 可以通过 js 控制 dom 来改变样式，@import 则不行
# 对语义化标签的理解
  语义化标签使得代码可读性更好，结构更清晰，有利于开发和维护。并且有利于SEO，搜索引擎爬虫也依赖语义化标签来确定上下文权重。在没有css的时候，语义化标签也使得内容结构更好被确定
  语义化标签：header  main  footer aside article section
# script标签中 async 和 defer 的区别
没有 async 和 defer  时，文档解析时遇到脚本会被阻塞，等脚本下载并执行完才会继续解析文档
* 写了defer ，那么遇到脚本时，就不会阻塞，脚本下载和文档解析共同进行，等文档解析完毕后，脚本才会执行，执行的顺序为 defer 的顺序
* 写了 async ，那么遇到脚本时也是脚本下载和文档解析同时进行，但是一旦脚本下载完毕，就会进行脚本的执行，执行的顺序不一定，取决于谁先下载完
# HTML 5 更新
`document.querySelector`、`document.querySelectorAll`
`video` 和 `audio`
`localStorage` 和 `sessionStorage`
`web worker`
`Notifications`
# 对 web worker 的理解
Web程序  => 主线程分离 => 后台线程  => 脚本操作  => 复杂任务  => 主线程不会阻塞 
# DOCTYPE 的作用是什么
html 第一行  =>  标准模式解析并渲染  =>  兼容模式  =>  模拟老旧浏览器,文档错乱
# 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？
行内： a   b   span   img   input    select    strong    button    label    textarea
块级： div  ul  ol  li  h1-h6  p  dl dt dd
空：`<br>`、`<hr>`、`<img>`、`<input>`、`<link>`、`<meta>`
#  img标签title、alt、srcset
title: 鼠标  =>  显示
alt: 加载失败
 srcset: 不同分辨率
# iframe
嵌入
优点：同时加载，代码复用
缺点: SEO，兼容性
# title与h1的区别、b与strong的区别、i与em的区别？
有层次的标题，语义加粗，强调斜体
#  head 标签有什么作用
文档头部 =>  所有头部元素容器  =>  描述属性和信息  =>  不显示给读者  => title, style, script, link =>  文档标题
# 标准模式与兼容模式各有什么区别
最高标准 ， 宽松向后兼容
# 布局模型
流动模型(flow), 浮动模型(float), 层模型(layer)