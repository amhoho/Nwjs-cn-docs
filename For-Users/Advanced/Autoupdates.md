# 自动更新

NWJS更支持社区内的更新解决方案,而非内置方案.以下是现有的相关方案:
- [node-webkit-updater](https://github.com/edjafarov/node-webkit-updater)
- [nwjs-autoupdater](https://github.com/oaleynik/nwjs-autoupdater)
- [nw-autoupdater](https://github.com/dsheiko/nw-autoupdater)

## node-webkit-updater

NPM模块为您提供了一些底层API:

- 检查当前程序版本.
- 如果存在新版本,则将新安装包下载并解压至temp临时目录.
- 杀死旧版进程并运行新版本程序,新版本文件将覆盖旧版本目录.
- 退出上述进程并启动新应用.

您可以参考这个根据上述逻辑的([构建范例](https://github.com/edjafarov/node-webkit-updater/blob/master/examples/basic.js)).      

## nwjs-autoupdater

这是个可内置于NWJS应用进行解压缩更新的仅有2M左右的golang小程序.

要更新目标应用,您需要将 `--bundle` 和 `--inst-dir`命令行参数传递给更新程序,改程序将在更新后重启应用.

- `--bundle` 内含新版本应用的zip的路径
- `--inst-dir` 是应用可执行文件的路径

与 `node-webkit-updater`相比有多个优点：

- 它可以对nwjs-autoupdater自身进行更新
- 无需提升权限 (除非应用本身安装在需要提权的目录中).
- 体量很小,所以无需在新版本中再次内置.

您可以参考这个根据上述逻辑的([构建范例](https://github.com/oaleynik/nwjs-autoupdater/blob/master/examples/index.js),该范例展示了如何将js模块作为NWJS应用入口点并在后买静默检查更新.

## nw-autoupdater

NPM模块提供了类似 `node-webkit-updater`的API,但其扩展更适合Node 7.x的NW.js,其async/await语法更加干净,该软件包内含了发布服务器和客户端的示例:

- 从远程服务器读取版本清单
- 检查清单中的版本是否大于当前版本
- 按系统平台下载匹配的最新版本 (根据远程清单中的 `packages`映射)
  - 订阅下载进度事件
-将其解压缩到临时目录(zip 或 tar.gz)
  - 订阅安装进度事件
- 关闭应用程序并将其作为分离的进程,并启动临时目录里下载的新版本
- 备份当前版本并替换为新版本
- 从应用原始目录重启已更新的应用程序