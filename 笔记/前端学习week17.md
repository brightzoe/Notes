---
title: 前端学习week17
date: 2019-08-25 22:52:05
comments: true
tags:
- javascript
- front-end
mathjax: false
---

本周主要学习了BOM和表单的内容，BOM是浏览器中和dom及原生js无关的api，主要是一些浏览器和网页的信息，表单是一些文件相关的内容，另外需要熟记节流和防抖的区别以及实现。

<!-- more -->

#### 防抖和节流

- 主要使用场景为函数高频调用时，例如监听mousemove事件等
- 防抖debounce
  - 主要思想是将频繁调用的函数用定时器延后执行，实际只执行一次
  - 两种思路
    - 第一次调用加入异步队列后忽略之后的调用
    - 记录定时器加入异步队列的id，每次调用都取消上一次定时，重新定时
- 节流throttle
  - 使连续的函数执行，变为间隔一段时间执行
  - 也是两种思路
    - 记录上一次执行的时间戳，若时间间隔大于设定间隔，则立即执行并记录当前时间戳
    - 设定定时器来调用，并记录是否设定了定时器，若定时器还存在则忽略调用，定时器调用时将状态改为未设置定时器 








### 表单

- textarea用标签间的文本作为初始值
- 文本框的change事件只会在输入框失焦才触发，可用键盘事件或input事件替代
  - 用js改变内容不会触发
- FileReader
  - 是一个构造函数
  - 先来了解一下file api
    - 表单中`<input type="file">`获取文件后，节点的files属性(FIleList对象)会包含读取的文件的集合类数组，集合内每个元素为一个file对象(构造函数是File，File的原型是blob)(可以认为是一个blob)，包含读取的文件的一些只读属性
  - 创建的对象实例可以读取文件的数据
  - 实例上的常用读取方法：
    - `readAsText()`参数为(file或Blob,encoding)，encoding指定编码类型，默认为UTF-8
    - `readAsDataURL(file或Blob)`
    - `readAsBinaryString(file或Blob)`
    - 读取的结果都保存在实例的result属性中
  - 文件读取完成时触发**实例上**的load事件
- URL.createObjectURL(File/Blob)
  - 创建一个blob url， 生命周期和创建它的窗口的document绑定，返回的url字符串指向指定的file/blob对象，可以在其他地方或窗口使用

#### localStorage

- window下的属性，同一个域名保存在本地同一个地方(同域的窗口才能获取)，关闭窗口不会清空，sessionStorage类似，但关闭窗口会清空
  - 不同浏览器当然存的不是一个地方，不能通用
- 以键值对的形式存储，都是文本格式
  - `setItem()`方法，两个参数，第一个为键，第二个为值，都为字符串形式(否则会自动转为字符串)，若想传入对象则一般转为JSON格式，取出时再转回
  - `getItem()`一个参数，键名
- storage事件
  - **只**发生在window，只有localStorage(sessionStorage不会)会触发此事件
  - 注意只有当存储的数据**实际修改**时才会触发，存储一个一摸一样的键值对不会触发，而且不会在导致数据改变的当前页面触发
  - 如果同时打开多个当前域名的窗口，如果某个页面修改了localStorage的数据，则其他页面的storage事件都会触发，而修改的页面不会触发，利用此特性可实现多窗口通信

### 其他事件

- resize 窗口大小变化，页面缩放也会触发

###  零碎知识

- **伪元素本身并不是DOM元素**，所以无法被js直接操作，也无法通过dom操作获取
  - [利用javascript获取并修改伪元素的值](https://segmentfault.com/a/1190000003711146)
- 鼠标按住左键不放移到窗口外仍能触发mousemove（拖动操作）
- `parseInt()`第一个参数是字符串，不是字符串也会转成字符串再解析，当字符串的第一个字符转成数字为NaN(或空字符串)时返回NaN，按顺序解析，当遇到第一个字符不能正确转为整数时则自动忽略此字符及之后的字符
  - **不要用parseInt替代Math.floor**
- 大多数时候直接向innHTML中添加script标签脚本不会执行，但创建节点后加入DOM中会执行
- [创建和触发event](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Events/Creating_and_triggering_events?tdsourcetag=s_pctim_aiomsg)
- 预编译，解决函数中各变量的值到底是什么的问题，按照以下步骤
  - 实际上预编译会先生成一个AO对象(activition object)，因为声明提升的存在，先找到所有声明和隐式声明(未申明直接赋值的隐式声明全局变量)的变量作为属性，值都设置为undefined
  - 将函数执行时传入的实参传给同名形参，使实参和形参统一
  - 找到函数声明，将函数体赋给对应的变量(注意代码中的赋值操作是在书写的位置执行的，例如`var a = 1`)
  - 正式执行时以AO为基础顺序执行代码
- a标签href属性使用`javascript:xxx`(Bookmarklet)不会匹配visited
- typeof 未申明变量返回undefined
- 记住对象默认转字符串调用对象原型上的方法 结果为`[object Object]`
- `obj.hasOwnProperty()`的参数是字符串或者Symbol
- 对象解构：
  - 形如`({a, b, c = 1} = {a: 1, b: 2})`，将右侧同名属性的值赋给左边
    - 右边对象不存在这个属性名则赋值为undefined(或者这个属性的值就是undefined)，在左边的属性名后写等号赋值可作为默认值作为解决办法(右边属性值是null的时候无效)
  - 表达式必须用`()`包裹，因为`{}`是语句块
  - 表达式的返回值为右边的对象，且当等号右边是null/undefined时会报错(因为不能读取null/undefined的属性)
  - 形如`({type: a} = {a: 1})`的语法`type: a`的意思是读取属性名为type的值a，并将右边对应的值赋给a
    - 可以利用这个语法赋值给不同名的变量，例如`({type: a} = {type: 1})`
  - 嵌套使用，检索对象里的属性并赋值