# 开发工具与调试

!!! "仅适用SDK构建方式"需知:
	开发工具只能在SDK构建方式中使用 , 参考[SDK构建](Advanced/Build-Flavors.md) . 推荐使用SDK构建方式进行开发和测试 , 其他构建方式发布应用 . 

## Open Developer Tools 开启开发工具

Windows和Linux系统中使用快捷键<kbd>F12</kbd>开启 , Mac系统中使用</kbd>+<kbd>&#8997;</kbd>+<kbd>i</kbd> . 

此外 , windows系统中可以使用[win.showDevTools()`](../References/Window.md#winshowdevtoolsiframe-callback-该方法仅支持sdk构建方式中使用)开启开发工具进行编程 . 

## Node.js模块

NW.js默认进行[独立环境模式](Advanced/JavaScript-Contexts-in-NW.js.md#独立环境模式)运行 . 调试Node.js模块需要在引用中右键并选择"Inspect Background Page" . 当运行到Node.js模块代码时 , 调试器会自动聚焦并暂停运行 . 

[混合环境模式](Advanced/JavaScript-Contexts-in-NW.js.md#混合环境模式)下 , Node.js模块可以在开发工具中直接进行调试 . 参考[JavaScript环境](Advanced/JavaScript-Contexts-in-NW.js.md) . 

## 远程调试

使用命令行参数 `--remote-debugging-port=port`指定端口进行监听 . 例如 , 运行 `nw --remote-debugging-port=9222` , 通过浏览器访问http://localhost:9222/进行远程调试 . 

## 开发工具扩展

开发工具支持全部扩展 , 包括ReactJS等 . 

使用扩展需要在配置文件manifest.json添加 `"chrome-extension://*"` 权限 , 并在nw运行时增加 `--load-extension=path/to/extension`命令行参数 . 扩展的文件从Chrome应用商店安装之后拷贝到本地Chrome浏览器目录下 . 

### 例子

[react-app](https://s3-us-west-2.amazonaws.com/nwjs/sample/react-app.zip)
[react-devtools](https://s3-us-west-2.amazonaws.com/nwjs/sample/react-devtools.zip)

解压文件 , 下载SDK构建方式的NW.js , 并运行 `nw.exe --load-extension=path/to/devtools path/to/app/folder`