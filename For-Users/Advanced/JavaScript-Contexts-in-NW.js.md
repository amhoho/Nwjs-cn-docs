# JavaScript环境
---

## JavaScript环境概念

脚本运行在不同窗口的JavaScript环境 . 例如 , 应用中每个窗口都拥有属于自己的全局对象以及全局结构( `Array` , `Object`) . 

多数浏览器惯用做法以及好的处理方式 , 如下:

* 对象属性替换 , 库扩展或者简单脚本(如[Prototype](http://prototypejs.org/)) , 不同窗口之间使用相似对象将不会受到影响 . 
* 单个窗口范围内程序错误(如[构造对象前缺少new](http://ejohn.org/blog/simple-class-instantiation/))或者bug不会影响到其他窗口 . 
* 其他窗口中恶意应用不能访问重要数据 . 

当脚本访问其他环境中的对象或者函数 , JS引擎将提供临时环境处理 , 完成之后立即离开 . 

## NW.js环境

NW.js基于Chrome应用构建 . 因此NW.js在开始运行时 , 自动完成后台加载 . 当创建一个窗口时 , 同时创建一个JavaScript环境 . 

NW.js中 , 默认情况下 , Node.js模块加载到后台运行环境 . 混合环境模式中 , 每个窗口或框架都加载在环境中 . 下文将阐述[独立环境模式](#separate-context-mode)和[混合环境模式](#mixed-context-mode)的差异.

## 独立环境模式

除了浏览器环境 , NW.js默认在后台增加Node环境运行Node模块 . 这样NW.js同时拥有两个JavaScript环境: **浏览器环境** 和 **Node环境** . 

!!! "Web Worker"需知:
	实际上Web Worker运行在独立的JavaScript环境 , 既非浏览器环境也非Node环境 . Web Worker无法访问Web或Node.js或NW.js API.

### 浏览器环境

#### 加载脚本
脚本通过传统的网络方式加载或嵌入,如 `<script>`标签或者jQuery的[`$.getScript()`](http://api.jquery.com/jQuery.getScript/)或者[RequireJS](http://requirejs.org/) . 

#### 全局对象

浏览器环境中 , 全局对象包括[JS内建对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)(如`Date` , `Error` , `TypedArray`)和[Web API](https://developer.mozilla.org/en-US/docs/Web/Reference/API) (如DOM API) . 

#### 创建新浏览器环境

每个窗口或frame有属于自己的环境 . 当创建新的frame或者窗口时 , 将得到一个新的浏览器环境 . 

#### 访问Node.js和NW.js的API

将Node环境对象拷贝到浏览器环境中 , 这样运行在浏览器环境的脚本能够访问Node.js对象 : 

* `nw` -- 所有NW对象 [NW.js APIs](../../References)
* `global` -- NW环境全局对象; 等同`nw.global`
* `require` -- 加载Node.js模块的方法; 等同 `nw.require()`, 但不支持通过 `require('nw.gui')`加载NW.js模块.
* `process` -- Node.js模块中[process模块](https://nodejs.org/api/globals.html#globals_process); 等同`nw.process`
* `Buffer` -- Node.js模块中[Buffer类](https://nodejs.org/api/globals.html#globals_class_buffer)  

#### `require()`中相对路径解析

浏览器环境中相对路径解析参考主页面的路径 . 

### Node环境

#### 加载脚本
Node运行环境加载脚本的方式:

* 通过Node.js的API中的`require()`加载脚本
* 通过[配置文件中`node-main`](../../References/Manifest-Format.md#node-main)加载脚本      

#### 全局对象

Node环境中运行的脚本能够使用 **JS内建对象** . 此外 , 还可以使用Node.js全局对象 , 如 `__dirname`, `process`, `Buffer`等 . 

注意,Node环境不能使用Web APIs . 参考[Node环境访问浏览器以及NW.js的API](#access-browser-and-nwjs-api-in-node-context)查找使用的方法                  

#### 创建新的Node环境
**独立环境模式中 , 所有的Node环境包含所有Node模块** . 但可以使用以下方法创建新的Node环境:

* 通过[`Window.open()`](../../References/Window.md#windowopenurl-options-callback)创建新窗口时 , 参数 `new_instance`设置为 `true` .
* 命令行启动NW.js时 , 参数增加 `--mixed-context`进入[混合环境模式](#mixed-context)           

#### Node环境访问浏览器API和NW.js的API

Node环境中 , 没有浏览器或NW.js APIs , 如 `alert()` , `document.*` , `nw.Clipboard`等 . 想访问浏览器APIs 必须传递相应的对象 , 如 `window` . 

有关如何实现这一点，请参见以下示例:

Node环境运行一下脚本(myscript.js):

```javascript
// 浏览器环境需要传入`el`
exports.setText = function(el) {
    el.innerHTML = 'hello';
};
```

浏览器(index.html):
```html
<div id="el"></div>
<script>
var myscript = require('./myscript');
// 将`el`元素传入Node函数
myscript.setText(document.getElementbyId('el'));
// "hello"将显示在`el`标签中
</script>
```

	Node环境中的 `window`对象指向后台中的DOM `window`对象 . 

#### `require()`方法中相对路径解析

Node模块中的相对路径相对于当前模块的路径 . 与Node.js中相对路径解析方式相同 . 

## 混合环境模式

当使用[`--mixed-context`](../../References/Command-Line-Options.md#mixedcontext)命令行参数运行NW.js时 , 当浏览器环境被创建的同时创建了Node环境 . 

### 混合环境模式中加载脚本

NW.js可以通过两种方式使用混合环境模式 , 一是使用`--mixed-context`命令行参数 , 二是在配置文件中添加[`chromium-args`](../../References/Manifest-Format.md#chromium-args)属性.

页面或Node.js中使用`require()`方式加载脚本运行在同样的环境中 . 

### 全局对象

混合环境模式 , 您可以在Node模块中使用所有浏览器和NW.js API，反之亦然

`package.json`
```javascript
{
    "name": "test-context",
    "main": "index.html",
    "chromium-args": "--mixed-context"
}
```

`myscript.js`
```javascript
exports.createDate = function() {
    return new Date();
};

exports.showAlert = function() {
    alert("我正在Node模块中运行!");
};
```

`index.html`
```html
<script>
var myscript = require('./myscript');

console.log(myscript.createDate() instanceof Date); // true
myscript.showAlert(); // 我正在Node模块中运行!
</script>
```

### 混合环境模式和独立环境模式对比

独立环境模式的优势是不会出现[类型检查问题](#working-with-multiple-contexts) . 

混合环境模式的缺点是不能轻易的分享变量 . 环境间分享变量 , 需要将变量放入其他环境能够访问的通用环境中 . 或者可以使用[`window.postMessage()` API](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage)在环境之间发送和接收信息 . 

## 多种环境混合使用

环境之间的差异通常是有利的 , 但有时可能会引起问题 . 

例如 , 不同浏览器环境中 , 全局对象并不相同 , 环境之间的类型检查会出错 . 

```html
<iframe id="myframe" src="myframe.html"></iframe>
<script>
// `window`是当前浏览器环境的全局对象
// `myframe.contentWindow`是`<iframe>`环境中的全局对象
var currentContext = window;
var iframeContext = document.getElementById('myframe').contentWindow;

// 当前环境定义的`myfunc`
function myfunc() {

}

console.log(currentContext.Date === iframeContext.Date); // false
console.log(currentContext.Function === iframeContext.Function); // false
console.log(myfunc instanceof currentContext.Function); // true
console.log(myfunc instanceof iframeContext.Function); // false
console.log(myfunc.constructor === currentContext.Function); // true
console.log(myfunc.constructor === iframeContext.Function); // false
</script>
```

### `instanceOf`问题

这类问题常发生在JavaScript中使用 `instanceof`操作过程 . 参考[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof) , `Value instanceof Constructor`操作测试对象是否在结构属性 `prototype`的原型链中 . 然而 , 如果不同JavaScript环境通过 `Value` , 而 `Value`有自己父类结构 , 那么 `Value instanceof Constructor`必然检查失败 . 

例如 , 简单的检查 `Value instanceof Array` , 在其他环境中能够通过 , 不能代表 `Value`变量的值是 `Array`中的值 . 

### `obj.constructor`问题

`obj.constructor`属性检查会引发问题 . 例如 , `Value.constructor === Array`被代替 `Value instanceof Array`使用 . 

### `obj.__proto__`问题

`obj.__proto__`访问对象属性会引发问题 . 比较全局对象构造函数或者使用 `instanceof`将会引起错误结果 . 

### 第三方库问题

第三方库可能引发以上类型检查问题 . 引发的问题可能不易查处原因 , 一旦发生 , 可能是由于第三方库引发 . 可以提交问题报告或者自行解决 . 

### 可靠的跨环境类型检查

当使用其他JavaScript环境中的值时 , 避免使用`instanceof`防止环境相关问题 . 

可以使用[`Array.isArray`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)方法检查值是否在数组中 , 这种方式比较可靠 . 

可以使用环境全局对象检查`Value`类型 , 例如`Function`或`Date`等 . 可以使用以下方式检查对象的真实类型 . 

```javascript
// 检查方法
Object.prototype.toString.apply(someValue) === "[object Function]"
// 检查日期
Object.prototype.toString.apply(someValue) === "[object Date]"
```

然而 , 没有现成的可以替代的方法 , 当面对问题是需要讨论之后进行修补 . 

Node环境中可以使用[`nwglobal`](https://github.com/Mithgol/nwglobal)获取全局对象进行类型检查 . 