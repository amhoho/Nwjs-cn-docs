# 配置文件
每个应用都有个名为`package.json`的[JSON](http://www.json.org/)格式的配置文件,本文将阐述该文档中配置的属性.

## 快速入门

最简单的配置文件内容:
```json
{
  "main": "index.html",
  "name": "nw-demo",
}
```

配置 `main`和 `name`即可启动可以NW.js应用:
* `main`:起始页,如 `index.html`也可以是 `.js`文件或url,该文件与 `package.json`统计目录.
* `name`: 给应用去个唯一的名称如 `nw-demo`

## 必要字段

每个配置文件中必须要提供以下属性:

### main
> 属性:**应用起始页**

* String - 如 `index.html`也可以是 `.js`文件或url,该文件与 `package.json`统计目录.

### name
> 属性:**应用名称,应确保该字段内容全局唯一性**

* String - 名称由数字 , 小写字母 , "." , "_"或者"-"组成,其它内容会被忽略.

## 扩展字段

控制NW.js应提供的功能及如何打开主窗口.

### nodejs
> 属性:**是否支持Node**

* Boolean - `false`时将禁用Node.

### node-main
> 属性:**指定Node.js脚本文件路径 . 并且它将在加载DOM窗口之前启动Node环境时执行**

* String - Node.js脚本文件路径

### domain
> 属性:**指定主机域名**

* String - 在 `chrome-extension://`协议URL指定主机域名 . WEB引擎将是应用和同一域名的网站之间共享cookie

### single-instance
> 属性:**是否以单实例运行**

* Boolean - `true`即单实例运行, `false`则允许应用多开,默认 `true`

### bg-script
> 属性:**后台脚本**

* String - 应用启动时执行的后台脚本 . 

### window
> 属性:**窗体样式控制**

* Object - 详见[窗体子字段](#窗口子字段)章节                

### webkit
> 属性:**WebKit特性控制**

* Object - 详见[WebKit子字段](#webkit子字段)章节         

### user-agent
> 属性:**重写应用请求页面中的 `User-Agent`信息**

* String - 重写的 `User-Agent`信息

以下变量内容可以动态设置 `User-Agent`内容:

* `%name`: 替换配置文件中的`name`字段 . 
* `%ver`: 替换配置文件中的`version`字段 . 
* `%nwver`: 替换NW.js版本 . 
* `%webkit_ver`: 替换WebKit引擎版本 . 
* `%osinfo`: 替换系统以及CUP信息 . 

### node-remote
> 属性:**从远程页面启用Node**

* Array 或 String - 数组中的各项均需遵循在Chrome扩展中使用的[匹配模式](https://developer.chrome.com/extensions/match_patterns)     

匹配模式本质上为URL , 由 `http`, `https`, `file` , `ftp`或者 `'*'`开始 . 其中 `'*'`代表匹配所有URL . 每个匹配模式由三部分组成:

基础语法:
```none
<url-pattern> := <scheme>://<host><path>
<scheme> := '*' | 'http' | 'https' | 'file' | 'ftp'
<host> := '*' | '*.' <any char except '/' and '*'>+
<path> := '/' <any chars>
```

### chromium-args
> 属性:**分发应用时自定义chromium命令行参数至应用**

* String - 指定的chromium命令行参数

例如:想要禁用GPU加速视频显示,只需添加添加参数 `"chromium-args" : "--disable-accelerated-video"`. 

如果添加多个参数,则使用空格进行分割,该字段也可使用单引号括起标记.命令行详细信息,请参阅 [Command-Line-Options](Command-Line-Options.md)

### crash_report_url
> 属性:**应用崩溃时,崩溃转存报告将被发送到设定的服务器**

* String - 崩溃报告服务器的URL

与Chromium浏览器发送方式相同,发送具有 `multipart/form-data`内容的HTTP POST请求.
理论上来讲, 任意 breakpad/crashpad 都可以处理该请求,因为 breakpad/crashpad 与NW相仿,. 请参阅 [简单服务器的小案例](https://github.com/nwjs/nw.js/blob/nw21/test/sanity/crash-dump-report/crash_server.py)或使用现有的[simple-breakpad-server](https://github.com/acrisci/simple-breakpad-server).

该请求至少包含以下字段:

* `prod` - 配置文件中的 `name` 属性
* `ver` - 配置文件中的 `version` 属性
* `upload_file_minidump` - minidump文件的二进制内容
* `switch-n` - 崩溃过程的命令行切换开关,每个切换有多个字段,其中 `n`从1起始.

### js-flags
> 属性:**指定JS引擎(V8)可用特性**

* String - 可用特性如打开协调代理( `Harmony Proxies`)以及集合( `Collections`):

```json
{
  "name": "nw-demo",
  "main": "index.html",
  "js-flags": "--harmony_proxies --harmony_collections"
}
```

### inject_js_start
### inject_js_end
> 属性:**执行JavaScript代码**

* String -相对于应用程序路径的本地文件名，期望执行的JavaScript文件

 `inject_js_start`: CSS文件执行之后 , 其他DOM或脚本运行之前 , 执行的JavaScript代码 . 

`inject_js_end`: 页面document对象加载之后 , 触发 `onload`之前 , 执行的JavaScript代码 . 主要作为新窗口中 `Window.open()`的参数执行JavaScript代码 . 

### additional_trust_anchors
> 属性:**证书作为附加可用的根证书使用 , 允许连接自签名证书或者CA签发机构颁发证书的服务**

* Array -  数多个PEM编码的证书组成的数组 , 例如 `"-----BEGIN CERTIFICATE-----\n...certificate data...\n-----END CERTIFICATE-----\n"`

### dom_storage_quota
> 属性:**Mb为单位的DOM存数限制数量**

* Integer - 建议设置为期望值的两倍

## 窗口子字段
> 属性:**窗口子字段默认情况被继承到使用 `window.open()`或 `<a target="_blank">`打开的子窗口 .未继承子字段将被设置为打开窗口时的默认值**

列表如下：
* `fullscreen` -> `false`
* `kiosk` -> `false`
* `position` -> `null`
* `resizable` -> `true`
* `show` -> `true`

所有窗口子字段可以使用[`new-win-policy`事件](Window.md#event-new-win-policy-frame-url-policy)重写 . 

### id
> 属性:**内含窗口尺寸与位置的状态的窗口ID,打开同ID的窗口时会还原该状态**
* String - 窗口ID,细节请另见[Chrome应用文档](https://developer.chrome.com/apps/app_window#type-CreateWindowOptions)        

### title
> 属性:**NW.js创建的窗口标题 . 在应用启动时显示的标题信息**

* String -标题

### width
### height
> 属性:**主窗口初始宽高**

* Integer - 宽高

### toolbar
> 属性:**是否显示导航栏中的工具条**

* Boolean - `true`显示, `false`不显示

### icon
> 属性:**窗口图标**

* String - 图标路径

### position
> 属性:**窗口位置**

* String - 默认 `null`(不固定) , `center`(屏幕居中) , `mouse`(鼠标所在位置)

### min_width
### min_height
> 属性:**窗口最小宽高**

* Integer - 最小宽高值

### max_width
### max_height
> 属性:**窗口最大宽高**

* Integer - 最大宽高值

### as_desktop_Linux_
> 属性:**X11环境下,作为桌面背景显示**

* Boolean - `true`显示, `false`不显示

### resizable
> 属性:**是否可调整窗口大小**

* Boolean - `true`允许, `false`不允许

注意,在OS X上将该属性设置为 `false`，并将frame设置为 `true`，用户还是可以将窗口全屏显示。只有将全屏也设置为 `false`才可禁用全屏控件。

### always_on_top
> 属性:**是否允许窗口始终置顶(在其余窗口之上)**

* Boolean -  `true`允许, `false`不允许

### visible_on_all_workspaces _Mac & Linux_
> 属性:**支持多工作区的系统(如Mac & Linux)中,将窗口同时显示在所有工作区中**

* Boolean -  `true`允许, `false`不允许

### fullscreen
> 属性:**是否允许窗口全屏**

* Boolean -  `true`允许, `false`不允许

注意,窗体和全屏框架应当一致,窗口设置为 `false`时,则全屏框架不应设为 `true`,避免窗体将阻止鼠标获取屏幕边缘.

### show_in_taskbar
> 属性:**是否允许显示在任务栏或停靠栏中**

* Boolean - `true`允许, `false`不允许,默认 `true`. 

### frame
> 属性:**窗口是否为框架**

* Boolean - 设为 `false`时即无框窗口.

注意,窗体和全屏框架应当一致,窗口设置为 `false`时,则全屏框架不应设为 `true`,避免窗体将阻止鼠标获取屏幕边缘.

### show
> 属性:**启动时是否显示应用**

* Boolean - `true`显示, `false`不显示

### kiosk
> 属性:**是否使用 `Kiosk`模式(该模式即应用将全屏并阻止用户离开应用,比如常见的公共触摸屏演示)**

* Boolean - `true`使用, `false`不使用

### transparent
> 属性:**窗口是否透明**

* Boolean - `true`允许, `false`不允许,默认 `false`. 

窗口的透明度由CSS中的背景透明值控制,

使用命令行参数 `--disable-transparency` 可完全禁止透明功能.

使用命令行参数 `--disable-gpu` 禁用GPU后,可实现透明窗体的穿透点击

## WebKit子字段

### double_tap_to_zoom_enabled
> 属性:**是否启用两指缩放功能**

* Boolean - `true`允许, `false`不允许,默认 `false`. 

### plugin
> 属性:**是否可加载扩展插件,比如Flash插件**

* Boolean - `true`允许, `false`不允许,默认 `true`. 

## 其他字段

 关于`package.json`的更多字段,请参考[Packages/1.0](http://wiki.commonjs.org/wiki/Packages/1.0)
 
### description
> 属性:**配置的描述说明,以`.`结束**

### version
> 属性:**应用版本信息**

### maintainers
> 属性:**维护成员**

```base
"maintainers":[ {
   "name": "Bill Bloggs",
   "email": "billblogs@bblogmedia.com",
  " web": "http://www.bblogmedia.com",
}]
```
### contributors
> 属性:**贡献者,与维护成员格式一致,但首个应为作者**

### bugs
> 属性:**提交错误的网址,如mail或http地址**

### licenses
> 属性:**许可证列表**
```base
"licenses": [
   {
       "type": "GPLv2",
       "url": "http://www.example.com/licenses/gpl.html",
   }
]
```
### dependencies
> 属性:**必要依赖,组的顺序非常重要,较前条目具有较高优先级.**

```base
"dependencies": {
       "webkit": "1.2",
       "ssl": {
           "gnutls": ["1.0", "2.0"],
           "openssl": "0.9.8",
       },
}
```
### homepage
> 属性:**网站URL地址**