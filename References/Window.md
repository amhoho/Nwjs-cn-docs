# 窗口
>   `Window` 是DOM中 顶层 `window`对象的包装类 . 可扩展操作以及接收窗口事件 . 

每个 `Window`都是EventEmitter类实例 , 使用 `Window.on(...)`可响应窗口事件 . 

## 示例

```javascript
//获取当前窗口
var win = nw.Window.get();

// 监听最小化事件
win.on('minimize', function() {
  console.log('Window is minimized');
});

// 窗口最小化
win.minimize();

// 移除最小化监听事件
win.removeAllListeners('minimize');

// 创建窗口并获取它的窗口对象
nw.Window.open('https://github.com', {}, function(new_win) {
  // 监听新窗口焦点事件
  new_win.on('focus', function() {
    console.log('New window is focused');
  });

});
```

## Window.get([window_object])
> 用途:**获取指定window_object窗口(未指定时默认为当前窗口)的窗口对象**

* `window_object` `{DOM Window}` (可选)  DOM窗口对象
* Returns `{Window}` the native `Window` object

如果 `window_object` 是 `iframe`, 该方法将返回其顶层窗口的 `Window` 对象.

```javascript
// 获取当前窗口的`Window`对象
var win = nw.Window.get();

/获取iframe的窗口
var iframeWin = nw.Window.get(iframe.contentWindow);

//返回true
console.log(iframeWin === win);

// 创建窗口并获取它的窗口对象
nw.Window.open('https://github.com/nwjs/nw.js', {}, function(new_win) {
  // do something with the newly created window
});
```

## Window.open(url, [options], [callback])
> 用途:**打开并加载 `url`至新窗口**

* `url` String - 加载至新开窗口的 `url`
* `options` Object - (可选)  属性详情请查看[窗口子字段](Manifest-Format.md#window-subfields):  
    - `new_instance` Boolean - (可选) 是否在单独的渲染过程中打开新窗口
    - `inject_js_start` String - (可选)  加载文档之前要执行的脚本,更多细节参阅 [Manifest format](Manifest-Format.md#inject_js_start)
    - `inject_js_end` String - (可选)  加载文档之后要执行的脚本. 更多细节参阅 [Manifest format](Manifest-Format.md#inject_js_end)
    - `id` String - (可选)  含窗口尺寸与位置的状态的窗口ID,打开同ID的窗口时会还原该状态,细节请另见[Chrome应用文档](https://developer.chrome.com/apps/app_window#type-CreateWindowOptions)        
    - `focus` Boolean - (可选) 是否聚焦打开的窗口.默认为 `false`.
* `callback(win)` Function - (可选)  窗口打开后的回调, `Window`对象将作为回调参数.
 
 与其任何组件进行交互都应在Window的 `loaded`事件之后.

## win.window
> 用途:**获取窗口中对应DOM的window对象**

## win.x
## win.y
> 用途:**获取或设置窗口在屏幕中距离左侧和顶部的偏移值**

## win.width
## win.height
> 用途:**获取或设置窗口尺寸**

## win.title
> 用途:**获取或设置窗口标题**

## win.menu
> 用途:**获取或设置窗口菜单栏**

通过 `type` 为 `menubar`对象完成, 当 `win.menu`设为 `null`即没有菜单

更多细节参阅 [自定义菜单栏](../For-Users/Advanced/Customize-Menubar.md)和[菜单](Menu.md).以及[菜单项](MenuItem.md)    

## win.isFullscreen
> 用途:**判断窗口是否为全屏模式**

## win.isTransparent 
> 用途:**判断窗口是否为使用透明**

## win.isKioskMode
> 用途:**判断窗口是否使用 `Kiosk`模式**

## win.zoomLevel
> 用途:**获取或设置页面缩放级别**

`0`表示正常大小 ; 数值为正数表示放大 ; 数值为负数表示缩小 . 

## win.cookies.*
> 用途:**窗口的Cookie 操作**

该内容包括了多个Cookie操作. 具体的Cookie API定义方式类同[Chrome Extensions'](http://developer.chrome.com/extensions/cookies.html). NW.js 支持 `get`, `getAll`, `remove` 和 `set` 方法以及 `onChanged` 事件,(在这个事件上还支持 `addListener`和 `removeListener`).

不过,在Chrome扩展API中的有关`CookieStore`的任意内容都不被支持,因为NW.js应用只有一个全局Cookie存储.

## win.moveTo(x, y)
> 用途:**移动窗口到指定位置,将窗口的左上角移动到指定的坐标**

* `x` Integer - 屏幕左侧偏移值
* `y` Integer - 屏幕顶部偏移值

## win.moveBy(x, y)
> 用途:**移动窗口到相对于当前窗口左上角的横纵偏移,比如窗口上移10px**

* `x` Integer - 横向偏移值
* `y` Integer - 纵向偏移值

## win.resizeTo(width, height)
> 用途:**重设窗口尺寸为 `width` 和 `height`**

* `width` Integer - 窗口宽度
* `height` Integer - 窗口高度

## win.resizeBy(width, height)
> 用途:**调整窗口尺寸为相对于当前窗口尺寸的`width` 和 `height`,比如窗口加宽10px**

* `width` Integer - 窗口宽度偏移值
* `height` Integer -窗口高度偏移值

## win.focus()
> 用途:**聚焦于窗口**

## win.blur()
> 用途:**从窗口中移除焦点,通常将焦点移至程序其它窗口,但某些系统平台没有 `blur`这个概念** 

## win.show([is_show])
> 用途:**是否显示指定窗口**

* `is_show` Boolean - (可选)  默认 `true`即显示

 `win.show(false)` 等同 `win.hide()`.

某些平台可能需要使用 `focus`手动聚焦窗口,因为显示后可能不会自动聚焦新窗口.

## win.hide()
> 用途:**隐藏窗口**

## win.close([force])
> 用途:**是否忽略 `close`事件或监听的强制关闭窗口**

* `force` Boolean - `true`强制关闭, `fasle`则不强制

例子:
```javascript
win.on('close', function() {
  this.hide(); // Pretend to be closed already
  console.log("We're closing...");
  this.close(true); // then close it forcely
});

win.close();
```

## win.reload()
> 用途:**重新加载当前窗口**

## win.reloadDev()
> 用途:**在新渲染进程重新加载当前页面**

## win.reloadIgnoringCache()
> 用途:**忽略缓存的重新加载当前页面**

类似 `reload()`但忽略了缓存(又称 `"shift-reload"`) . 

## win.maximize()
> 用途:**Linux和Window中最大化窗口 , Mac中放大窗口**

## win.minimize()
> 用途:**Windows/Mac中最小化窗口 , Linux中图标化窗口**

## win.restore()
> 用途:**还原窗口,与最小化相反,更多人使用 `restore`来还原最大化的窗口**

## win.enterFullscreen()
> 用途:**窗口进入全屏模式,但不同于HTML5的FullScreen API的部分页面全屏,该方法会使整个窗口进入全屏**

## win.leaveFullscreen()
> 用途:**窗口退出全屏模式**

## win.toggleFullscreen()
> 用途:**窗口切换全屏模式**

## win.enterKioskMode()
> 用途:**窗口进入 `Kiosk`模式(该模式即应用将全屏并阻止用户离开应用,比如常见的公共触摸屏演示)**

## win.leaveKioskMode()
> 用途:**窗口退出 `Kiosk`模式**

## win.toggleKioskMode()
> 用途:**窗口切换 `Kiosk`模式**

## win.showDevTools([iframe], [callback]) _该方法仅支持SDK构造方式中使用_
> 用途:**打开调试工具**

* `iframe` String 或 HTMLIFrameElement - (可选) 要被检查的`<iframe> `的id或Element. 调试工具默认以独立窗口显示
* `callback(dev_win)` Function - 开发调试工具窗的带有 `Window`对象为参数 `dev_win`的的回调

 `iframe`为 `String` 即 `<iframe> `标签的 `id`,开发工具将检查 `<iframe> `内容,为空则无效.

 `iframe`为 `HTMLIFrameElement` 即 iframe 对象.作用同上.

有关如何在webview中或为webview打开调试工具,请参阅[webview 标签](webview-Tag.md)  

## win.closeDevTools() _该方法仅支持SDK构造方式中使用_
> 用途:**关闭调试工具**

## win.getPrinters(callback)
> 用途:**枚举系统中可用的打印机信息并在回调里发送json格式的打印机信息数组**

JSON对象中的设备名可使用于 `nw.Window.print()`方法中

## win.isDevToolsOpen() _该方法仅支持SDK构造方式中使用_
> 用途:**判断开发工具是否已打开,细节详见[`win.showDevTools()`](#winshowdevtoolsiframe-callback)**

## win.print(options)
> 用途:**静默方式(无需用户参与或交互)打印页面内容,如打印小票**

* `autoprint` Boolean - (可选) 是否以静默方式,默认为 `true`
* `printer` String - 由 `nw.Window.getPrinters()`返回的打印机设备名称; 该项在打印PDF时无需设置.
* `pdf_path` String - 在打印到PDF时用来存放PDF的路径
* `headerFooterEnabled` Boolean -  是否启用页眉和页脚
* `landscape` Boolean - 是否使用横向或纵向
* `mediaSize` JSON Object - 纸张尺寸规格
* `shouldPrintBackgrounds` Boolean - 是否打印CSS背景
* `marginsType` Integer - `0`为默认边距; `1`为无边距; `2`为最小边距; `3`为自定义边距, 用法参阅下文 `marginsCustom`.
* `marginsCustom` JSON Object - 自定义边距设置,单位为px
* `copies` Integer - 打印份数
* `headerString` String - 用于替换页头URL的内容
* `footerString` String - 用于替换页脚URL的内容

 `marginsCustom` 示例: `"marginsCustom":{"marginBottom":54,"marginLeft":70,"marginRight":28,"marginTop":32}`  
 `mediaSize` 示例: `'mediaSize':{'name': 'CUSTOM', 'width_microns': 279400, 'height_microns': 215900, 'custom_display_name':'Letter', 'is_default': true}`

如果您无需设置参数,请直接使用 `win.print({})` .

## win.setMaximumSize(width, height)
> 用途:**设置窗口的最大宽高**

* `width` Integer - 窗口最大宽度
* `height` Integer - 窗口最大高度

## win.setMinimumSize(width, height)
> 用途:**设置窗口的最小宽高**

* `width` Integer - 窗口最小宽度
* `height` Integer - 窗口最小高度

## win.setResizable(resizable)
> 用途:**是否允许调整窗口大小**

* `resizable` Boolean - `true`允许, `false`不允许

## win.setAlwaysOnTop(top)
> 用途:**是否允许窗口总是置顶(在其余窗口上面)**

* `top` Boolean -  `true`允许, `false`不允许

## win.setVisibleOnAllWorkspaces(visible) (Mac and Linux)
> 用途:**是否允许窗口在所有工作区中都显示**

* `visible` Boolean - `true`允许, `false`不允许

支持多工作区的系统(如Mac & Linux)中,将窗口同时显示在所有工作区中

## win.canSetVisibleOnAllWorkspaces() (Mac and Linux)
> 用途:**判断所在系统是否支持所有工作区窗口可见( `Boolean`)**

该方法目前不支持Window,Window请使用 `setVisibleOnAllWorkspace(Boolean)` 方法

## win.setPosition(position)
> 用途:**移动窗口到指定位置**

* `position` String -指定的窗口位置,可选值:  `null`(不固定) , `center`(屏幕居中) , `mouse`(鼠标所在位置)

## win.setShowInTaskbar(show)
> 用途:**是否允许窗口在任务栏或dock中显示**

* `show` Boolean - `true`允许, `false`不允许

该功能也可参考[配置文件](Manifest-format](Manifest-Format.md#show_in_taskbar)的 `show_in_taskbar`属性

## win.requestAttention(attension)
> 用途:**设置在任务栏中的窗口进行闪烁/禁止闪烁/闪烁次数**

* `attension` Boolean或Integer - Boolean表示闪烁或禁止闪烁. Integer表示闪烁次数

Mac中 `attension`> 0即 `NSInformationalRequest`, `attension`<0即 `NSCriticalRequest`.

## win.capturePage(callback [, config ])
> 用途:**窗口可视区域的页面截图**

* `callback` Function -页面截图后的回调
* `config` String或Object - (可选)  截图设置, 如果String则可选值为 `"png"` 和 `"jpeg"`,如果Object则为如下:
    - `format` String - (可选) 生成的图片格式,可选: `"png"` 和 `"jpeg"`. 默认为 `"jpeg"`
    - `datatype` String - 数据类型,可选: `"raw"`(Base64编码), `"buffer"`(缓存) 和 `"datauri"`(可用 `<src> `标签加载). 默认为 `"datauri"`

例子:
```javascript
//png图片的base64编码
win.capturePage(function(base64string){
 // 使用base64编码字符串处理某些需求
}, { format : 'png', datatype : 'raw'} );

// png图片为buffer对象
win.capturePage(function(buffer){
// 使用buffer对象处理某些需求
}, { format : 'png', datatype : 'buffer'} );
```

## win.setProgressBar(progress)
> 用途:**设置任务栏或dock上的进度条**

* `progress` Float -  进度值为[0, 1]之间. 当值为负数即删除该进度条.

注意,Linux中只有Ubuntu支持该项,但需要配置 `.desktop`文件的 `NW_DESKTOP`环境变量,未配置则默认使用 `nw.desktop`

## win.setBadgeLabel(label)
> 用途:**设置任务栏或dock上的图标标签**

注意,Linux中只有Ubuntu支持该项,但该项仅为数字类型字符串,且需要配置 `.desktop`文件的 `NW_DESKTOP`环境变量,未配置则默认使用 `nw.desktop`

## win.eval(frame, script)
> 用途:**在指定 `frame`中执行评估代码 `script`**

* `frame` HTMLIFrameElement - 要评估的框架. If `iframe` 为 `null`即作用于当前窗口或frame.
* `script` String - 要评估的js代码

## win.evalNWBin(frame, path)
> 用途:**在指定 `frame`中执行评估指定 `path`的JavaScript保护文件**

* `frame` HTMLIFrameElement -要评估的框架. If `iframe` 为 `null`即作用于当前窗口或frame.
* `path` String|ArrayBuffer|Buffer - 通过 `nwjc`生成的 `Buffer` 或 `ArrayBuffer` 文件(JavaScript保护文件的路径 )

有关JavaScript保护文件,详见[V8编译保护JavaScript代码](../For-Users/Advanced/Protect-JavaScript-Source-Code.md)          

## win.removeAllListeners([eventName])
> 用途:**移除所有或者指定 `eventName`的监听**

## 事件: close
> 触发:**窗口关闭后,如A窗口监听着B窗口的关闭事件,B窗口关闭时将触发本事件**

 如果对窗口发起了 `close`事件监听,那么需要进行以下的流程:
 1. `Window.close()`会先触发 `close`事件进行处理相关工作.
 2. 触发后请先对窗口进行必要的 `.hide()`隐藏,使用户觉得窗口立即被关闭了而不影响用户体验.
 3. 再调用 `this.close(true)`进行真正关闭.注意 `true`是必须的,如果忘记了 `true`会陷入无限循环
 
 有关示范例子,请查看[这里](#wincloseforce)  

Mac中 , 使用<kbd> &#8984;</kbd> +<kbd> Q</kbd> 关闭窗口时 , 会传递个指示字符串给 `close`的回调 , `quit`即成功关闭, 否则为 `undefined` . 

## 事件: closed
> 触发:**窗口关闭后,如A窗口监听着B窗口的关闭事件,B窗口关闭时将触发本事件**

如果你只在一个窗口处理 `closed`,并不能得到该事件,因为 `closed`后,所有js对象均被释放.

但在另一个窗口中监听此窗口的 `closed`时,那么js对象就不会被释放并将触发此窗口的 `closed`.

```javascript
// 打开一个新窗口
nw.Window.open('popup.html', {}, function(win) {
// 新窗口关闭后释放'win'对象
win.on('closed', function() {
  win = null;
});

// 监听主窗口的`close`事件
nw.Window.get().on('close', function() {
  // 关闭时先进行隐藏以让用户觉得立即关闭
  this.hide();

  // 虽然关了,但实际上它还在工作
  if (win != null)
    win.close(true);

  // 关闭新窗口也关闭主窗口
  this.close(true);
});

});

```

## 事件: loading
> 触发:**窗口加载时,如A窗口监听着B窗口的`loading`事件,B窗口进行了刷新或重载**

由于需要在设置回调方法之前触发该事件,所以在窗口开始加载时无法触发 `loading`.

除非,其它窗口正在监听该窗口的 `loading`时,该窗口进行了刷新或重载.

## 事件: loaded
> 触发:**窗口加载完成后**

该事件行为与 `window.onload`类似,但无需依赖DOM.

## 事件: document-start(frame)
> 触发:**窗口文档对象或者子框架可用且所有文件加载之后,构建DOM或运行任何脚本之前**

* `frame` HTMLIFrameElement -  iframe对象 , 如果为`null`表示窗口事件 . 

通过 `nw.Window.open()`创建的新窗口并不会触发这个事件且会解除事件绑定的回调函数.

参考[配置文件](Manifest-Format.md#inject-js-start)中 `inject-js-start`字段 . 

## 事件: document-end(frame)
> 触发:**窗口文档对象或者卸载时, `onunload`事件被触发之前**

* `frame` HTMLIFrameElement -  iframe对象 , 如果为`null`表示窗口事件 . 

参考[配置文件](Manifest-Format.md#inject-js-end)中 `inject-js-end`字段 . 

## 事件: focus
> 触发:**窗口获得焦点时**

## 事件: blur
> 触发:**窗口失去焦点时**

## 事件: minimize
> 触发:**窗口最小化时**

## 事件: restore
> 触发:**从最小化,最大化或全面模式进行荒原时,如最大化还原时**

## 事件: maximize
> 触发:**窗口最大化时**

## 事件: move(x, y)
> 触发:**窗口移动时(将左上角的左侧和顶端距离坐标 `(x, y)`传至回调)**

## 事件: resize(width, height)
> 触发:**窗口调整大小时(将新尺寸宽度和高度 `(width, height)`传至回调)**

## 事件: enter-fullscreen
> 触发:**窗口进入全屏时**

## 事件: zoom
> 触发:**窗口缩放时**
该事件需要提供缩放等级参数(`0`表示正常大小 ; 数值为正数表示放大 ; 数值为负数表示缩小 . )

## 事件: devtools-closed
> 触发:**调试工具被关闭时**

更多细节参阅 [`win.closeDevTools()` 方法](#winclosedevtools) 

## 事件: new-win-policy (frame, url, policy)
> 触发:**该窗口内或子框架中打开新窗口时**

* `frame` HTMLIFrameElement - 要处理的子框架对象 , 为 `null`表示顶层窗口 . 
* `url` String - 请求的链接地址 .
* `policy` Object - 改变打开新窗口的默认行为,如下:
    * `ignore()` : 忽略请求
    * `forceCurrent()` : 在同一框架中打开
    * `forceDownload()` : 链接是可下载的，或者由外部程序打开
    * `forceNewWindow()` : 新窗口中打开
    * `forceNewPopup()` : 新弹出窗口中打开
    * `setNewWindowManifest(m)` : 控制新弹出窗口中的参数 . `m`对象格式等同配置文件中[Window子字段](Manifest-Format.md#window-subfields)    

例如使用系统默认浏览器打开指定URL:

```javascript

nw.Window.get().on('new-win-policy', function(frame, url, policy) {
  // 不打开窗口
  policy.ignore();
  // 在系统默认浏览器打开
  nw.Shell.openExternal(url);
});

```

## 事件: navigation (frame, url, policy)
> 触发:**导航到其他页面时**

* `frame` HTMLIFrameElement - 要处理的子框架对象 , 为 `null`表示顶层窗口 . 
* `url` String - 请求的链接地址 .
* `policy` Object - 可忽略导航,如下:
    * `ignore()` : 忽略请求

用法类同上文[`new-win-policy`](#event-new-win-policy-frame-url-policy)