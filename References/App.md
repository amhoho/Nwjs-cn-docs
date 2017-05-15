# App

# 属性列表

## App.argv
> 用途:**应用启动时,返回应用自身所必须的命令行参数**

在NW.js中, 有些命令行是提供给NW.js使用的, 而应用本身并没有使用它们. 

## App.fullArgv
> 用途:**应用启动时,返回包括应用和NW.js使用的所有命令行参数**
 
比如 `--nwapp`, `--remote-debugging-port` 等等.

## App.filteredArgv
> 用途:**应用启动时,返回被过滤的应用非必需的参数**
 
默认 , 以下参数将会过滤掉:
```javascript
[
  /^--url=/,
  /^--remote-debugging-port=/,
  /^--renderer-cmd-prefix=/,
  /^--nwapp=/
]
```

## App.dataPath
> 用途:**获取应用数据使用文件的路径**
 
* Windows: `%LOCALAPPDATA%/<name>`
* Linux: `~/.config/<name>`
* OS X: `~/Library/Application Support/<name>/Default` (was `~/Library/Application Support/<name>` in v0.12.3 and below)

 `<name>` 指的是 `package.json` 配置文件中的**name**属性

## App.manifest
> 用途:**获取配置文件的JSON格式对象**
 
# 方法列表

## App.clearCache()
> 用途:**清空HTTP内存以及磁盘中的缓存**
 
该方法为异步方式调用 . 

## App.clearAppCache(manifest_url)
> 用途:**通过配置URL清空指定缓存**
 
该方法为异步方式调用 . 

## App.closeAllWindows()
> 用途:**关闭所有窗口并退出应用**
 
将 `close` 事件发送给应用的所有窗口 . 如果没有窗口阻塞close事件 , 那么在所有窗口完成关闭后，应用程序将退出.这样一来, 所有窗口都有一次保存数据的机会 . 

## App.crashBrowser()
> 用途:**使浏览器进程崩溃**

## App.crashRenderer()
> 用途:**使渲染器进程崩溃**
 
涉及崩溃机制使用的两个方法 , 参考[崩溃机制](../For-Developers/Understanding-Crash-Dump.md)特性.     

## App.getProxyForURL(url)
> 用途:**通过DOM中加载的 `url` 查询代理信息**
 
* `url` String -  通过URL查询代理信息

返回结果格式与[PAC](http://en.wikipedia.org/wiki/Proxy_auto-config)相同. (例如. "DIRECT", "PROXY localhost:8080").

## App.setProxyConfig(config, pac_url)
> 用途:**网页引擎将使用设置的代理配置请求网络资源或通过PAC url自动检测代理**
 
* `config` String -  代理配置
* `pac_url` String -  PAC url

规则拷贝自[`net/proxy/proxy_config.h`](https://github.com/nwjs/chromium.src/blob/nw13/net/proxy/proxy_config.h)文档

```
     // 一个字符串解析规则说明代理使用方式 . 
    //
    //   proxy-uri = [<proxy-scheme>"://"]<proxy-host>[":"<proxy-port>]
    //
    //   proxy-uri-list = <proxy-uri>[","<proxy-uri-list>]
    //
    //   url-scheme = "http" | "https" | "ftp" | "socks"
    //
    //   scheme-proxies = [<url-scheme>"="]<proxy-uri-list>
    //
    //   proxy-rules = scheme-proxies[";"<scheme-proxies>]
    //
    // 因此 , 代理规则字符串应该通过分号分割有序的代理 , 应用到特定的URL协议 . 
    // 无特殊原因 , 代理协议默认为http . 
    //
    // 特殊情况:
    //  * 代理列表第一个协议被删除 , 那么代理列表之后的协议都将被忽略 . 
    //  * 如果代理列表中的任意协议被删除 , 该列表中的其他协议不会被忽略 . 
    //  * 如果协议设置为'socks' , 那么所有协议都需要指定协议方式 , 如果没有指明将会认为`socks4://` . 
    //
    // 例子:
    //   "http=foopy:80;ftp=foopy2"  -- http的URL使用HTTP代理"foopy:80" , ftp的URL使用HTTP代理"foopy2" . 
    //   "foopy:80"                  -- 所有URL使用HTTP代理"foopy:80" . 
    //   "foopy:80,bar,direct://"    -- 所有URL使用HTTP代理"foopy:80" , "foopy:80"不可用使用"bar" , 直至没有可用代理为止 . 
    //   "socks4://foopy"            -- 所有URL使用SOCKS v4代理"foopy:1080"
    //   "http=foop,socks5://bar.com -- http的URL使用HTTP代理"foop" , "foop"不可用使用SOCKS5代理"bar.com"
    //   "http=foopy,direct://       -- http的URL使用HTTP代理"foopy" , "foop"不可用将不使用代理
    //   "http=foopy;socks=foopy2   --  http的URL使用HTTP代理"foopy" , 其他URL使用SOCKS4代理foopy2
```

## App.quit()
> 用途:**不发送 `close`事件给窗口,直接静默退出应用**

## App.addOriginAccessWhitelistEntry(sourceOrigin, destinationProtocol, destinationHost, allowDestinationSubdomains)
> 用途:**添加跨域入口地址的白名单**
 
* `sourceOrigin` String - 源地址 , 例如 `http://github.com/`
* `destinationProtocol` String -  `sourceOrigin` 可访问的目标协议 , 例如 `app`
* `destinationHost` String -  `sourceOrigin`可访问的目标主机 , 例如 `myapp`
* `allowDestinationSubdomains` Boolean -  设为 `true` 表示 `sourceOrigin` 可访问目标地址的子域(跨域).

假设应用通过从 `github.com`重定向到应用页面 , 使用该方法如下:

```javascript
App.addOriginAccessWhitelistEntry('http://github.com/', 'chrome-extension', location.host, true);
```

使用完全相同参数的 `App.removeOriginAccessWhitelistEntry`则从白名单中移除.

## App.removeOriginAccessWhitelistEntry(sourceOrigin, destinationProtocol, destinationHost, allowDestinationSubdomains)
> 用途:**从跨域入口地址的白名单中移除**
 
* `sourceOrigin` String - 源地址 , 例如 `http://github.com/`
* `destinationProtocol` String -  `sourceOrigin` 可访问的目标协议 , 例如 `app`
* `destinationHost` String -  `sourceOrigin`可访问的目标主机 , 例如 `myapp`
* `allowDestinationSubdomains` Boolean -  设为 `true` 表示 `sourceOrigin` 可访问目标地址的子域(跨域).

## App.registerGlobalHotKey(shortcut)
> 用途:**注册全局快捷键(全系统热键)**
 
* `shortcut` [Shortcut](Shortcut.md) - 注册的快捷键 `Shortcut`对象

## App.unregisterGlobalHotKey(shortcut)
> 用途:**注销全局快捷键(全系统热键)**
 
* `shortcut` [Shortcut](Shortcut.md) - 注销的快捷键 `Shortcut`对象

# 事件列表

## 事件: open(args)
> 触发:**应用打开文件时**
 
* `args` String - 程序的完整命令行

## 事件: reopen _Mac_
> 触发:**应用已运行而点击Dock时**