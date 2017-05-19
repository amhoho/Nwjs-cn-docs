# 快速入门
> NW.js基于[Chromium](http://www.chromium.org) 和 [Node.js](http://nodejs.org/). NW.js利用Web技术结合Node.js及其模块进行桌面应用开发.

## 获取 NW.js

您可以从[官网](http://nwjs.io)获取最新版本的文件,或通过[源代码构建](../For-Developers/Building-NW.js.md).         

!!! 注意 :
    推荐选择SDK构建方式开发应用 , 这样就可以使用开发工具进行调试 , 参考[构建方式](Advanced/Build-Flavors.md)章节查看构建方式之间的区别.

## 开发NW.js应用

### 例子 1 - Hello World

快速开发一个NW.js应用.

 **步骤 1.** 创建 `package.json`:
 
```json
{
  "name": "helloworld",
  "main": "index.html"
}
```

`package.json`是[JSON 格式](http://www.json.org/)格式的配置文件.  `main` 属性定义了应用首页, 如本例的 `"index.html"`. `name`则定义了应用名称. 具体查看 [配置文件](../References/Manifest-Format.md)章节.   

!!!  "main属性定义为js文件"需知:
    您可以将 `"main"`属性设置如 `"main.js"`的js文件. 该文件在应用启动时默认不打开窗口并在后台执行。 您可以稍后进行一些初始化并手动打开窗口。 例如:

```javascript
// 初始化你的应用程序之后 ...
nw.Window.open('index.html', {}, function(win) {});
```
    
 **步骤 2.** 创建 `index.html`文件:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

 **步骤 3.**  运行应用

```bash
cd /path/to/your/app
/path/to/nw .
```

 `/path/to/nw` 是NW.js执行文件. 
* Windows系统中是 `nw.exe`; 
* Linux系统中是 `nw`; 
* Mac系统中是 `nwjs.app/Contents/MacOS/nwjs`; 

!!! "Windows中的拖拽"需知:
    Windows系统中 , 可拖拽包含 `package.json`的文件夹至 `nw.exe`直接运行应用.

### 例子 2 - 使用 NW.js APIs

NW.js中的APIs都被加载到 `nw`全局对象中 , 并能够在JavaScript代码中直接使用 . 更多细节请参考[API文档](../References)   

本例子以 `index.html`为例展示了如何创建应用菜单:

```html
<!DOCTYPE html>
<html>
<head>
  <title>上下文菜单</title>
</head>
<body style="width: 100%; height: 100%;">

<p>右击显示上下文菜单</p>

<script>
// 创建一个空菜单
var menu = new nw.Menu();

// 添加菜单项
menu.append(new nw.MenuItem({
  label: '项 A',
  click: function(){
    alert('You have clicked at "项 A"');
  }
}));
menu.append(new nw.MenuItem({ label: '项 B' }));
menu.append(new nw.MenuItem({ type: 'separator' }));
menu.append(new nw.MenuItem({ label: '项 C' }));

// 监听事件
document.body.addEventListener('contextmenu', function(ev) {
  // 阻止显示默认菜单
  ev.preventDefault();
  // 点击处弹出定义的菜单对象
  menu.popup(ev.x, ev.y);

  return false;
}, false);

</script>  
</body>
</html>
```

... 运行应用:
```bash
cd /path/to/your/app
/path/to/nw .
```

!!! "require('nw.gui')"需知:
同样支持旧版加载 `nw`方式 `require('nw.gui')` , 返回 `nw`对象 . 

### 例子 3 - 使用 Node.js API 

你可以使用DOM直接调用node.js代码以及模块 . 这样就可以通过Nw.js轻松开发桌面应用 . 

本例子以 `index.html`为例展示了利用Node.js的 `os`模块查询操作系统信息:

```html
<!DOCTYPE html>
<html>
<head>
  <title>My OS Platform</title>
</head>
<body>
<script>
// 使用node.js获取系统信息
var os = require('os');
document.write('您当前系统为 ', os.platform());
</script>
</body>
</html>
```

您可以在在NW.js中通过[`npm`](https://www.npmjs.com/) 进行模块的安装 . 

!!! "Node原生模块"需知:
    当使用 `npm install`构建原生模块时无法兼容 NW.js ABI. 如需使用则需要根据 [`nw-gyp`](https://github.com/nwjs/nw-gyp)进行NW.js的重建. 详细参考[使用原生模块](Advanced/Use-Native-Node-Modules.md)章节.          

## 下一步该做什么

参考[开发工具与调试](Debugging-with-DevTools.md)进行开发以及调试应用 . 

参考[打包与发布](Package-and-Distribute.md)进行开发以及调试应用 . 

参考[FAQ](FAQ.md)解决你可能遇到的问题 . 

## 帮助

[NW.js wiki](https://github.com/nwjs/nw.js/wiki)中提供了很多干货,可能很有帮助.

您可以在[mail list on Google group](https://groups.google.com/forum/#!forum/nwjs-general)或 [Gitter](https://gitter.im/nwjs/nw.js)留言.

Bugs和需求可以提交到[GitHub](https://github.com/nwjs/nw.js/issues)
