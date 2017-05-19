# 命令行参数
> 启动NW.js时，您可以使用以下命令行参数来更改某些默认行为。

## 关于命令行参数

当用户使用应用时如命令行中打开文件: `your-app file.txt file2.txt`, `file.txt file2.txt`将会被记录下来,你可以使用[nw.App.argv](App.md)获得该命令行参数组成的数组. 

如果应用以多个实例运行,那么当前已运行一个实例时,将`open`事件传递给 `App` 对象可将第二个实例的完整命令行传递给现有实例

特别注意,为了支持 `child_process.fork()` API.如果命令行参数是磁盘上的.js文件的文件吗,那么NW将仅以Node.js方式运行.

## `--url`

加载URL到应用中,如: `--url=http://nwjs.io`

## `--mixed-context`

NW.js使用[混合环境模式](../For-Users/Advanced/JavaScript-Contexts-in-NW.js.md#mixed-context-mode) 代替独立环境模式运行 . 

## `--nwapp`

指定应用程序路径的另一种方法. 这个方法在 [ ChromeDriver测试](../For-Users/Advanced/Test-with-ChromeDriver.md)时非常有用.

## `--user-data-dir`

指定应用的数据目录，其中包含存储的数据，缓存和崩溃转储等。各系统中默认数据目录如下：

* Windows: `%LOCALAPPDATA%/<name-in-manifest>/`
* Mac: `~/Library/Application Support/<name-in-manifest>/`
* Linux: `~/.config/<name-in-manifest>`

 `<name-in-manifest>`为配置文件中[`name`](Manifest-Format.md#name)属性 . 

## `--disable-devtools`

SDK构建方式中禁止用户访问开发工具。

## `--enable-node-worker`

在Web Workers中集成Node.js.  在通过[结构化克隆](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm)算法使用DOM[大量交换数据](https://developer.mozilla.org/en-US/docs/Web/API/Worker/postMessage)的时候,可使用它通过新线程卸载CPU占用的任务.

请注意,Node.js模块需要安全线程中才能这样使用,虽然我们对Node.js内核不断进行改进使之确保线程安全.但我们无法限制第三方模块也做到这一点

## `--disable-raf-throttling`

启用这个参数时, 最小化或隐藏窗口时 将触发 `requestAnimationFrame()`回调. 这对游戏开发者而言是非常有用的,但它不启用时,其行为与浏览器无差别.

## `--disable-cookie-encryption`

默认情况下,cookie被加密存放在Chromium中,启用该参数可禁用cookie加密以便测试(如不同平台间的cookie共享).

## `--disable-crash-handler=true`

启用这个参数时, 将禁用单进程模式的崩溃处理进程,如果它与`--single-process`一起启用时,应用将只有一个NW进程.该参数将禁用崩溃存储功能

## `--enable-gcm`
## `--enable-transparent-visuals`
## `--disable-transparency`
## `--disable-gpu`
## `--force-cpu-draw`

以上几个选项均与透明窗口相关,细节请参考[透明窗口](../For-Users/Advanced/Transparent-Window.md)章节      

## 其他Chromium参数

请参考这些文档:

 [chrome_switches](https://github.com/nwjs/chromium.src/blob/nw18/chrome/common/chrome_switches.cc)
 
 [content_switches](https://github.com/nwjs/chromium.src/blob/nw18/content/public/common/content_switches.cc)
 
 [chromium-command-line-switches](http://peter.sh/experiments/chromium-command-line-switches/)
 
Chromium参数可以添加到配置文件[`chromium-args`](Manifest-Format.md#chromium-args)的 `chromium-args`属性中以作为NW.js运行时的参数 . 