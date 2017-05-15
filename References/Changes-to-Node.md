# Node变化

## console

从NW.js支持GUI应用而不是控制台应用,所以  `console.log()` (类似的还有 `console.warn()` 和 `console.error()`) 将重定向到Chromium的控制台. 您可以在"[开发者调试工具](../For-Users/Debugging-with-DevTools.md)"中的"控制台"选项卡中看到.

## process

`process`对象中增加了一些新属性:

* `process.versions['nw']` - NW.js版本.
* `process.versions['chromium']`  - NW.js所基于Chromium版本 .
* `process.mainModule`  - 起始页如 `index.html` , 配置文件中的[`main`](Manifest-Format.md#main)属性 . 但是当配置文件里还设置了[`node-main`](Manifest-Format.md#node-main)属性时 , `process.mainModule`值将为 `node-main`设置的属性 . 

## require

Node的 `require()`方法中的相对路径行为 , 依赖被调用的父类文件运行在哪个[JavaScript环境](../For-Users/Advanced/JavaScript-Contexts-in-NW.js.md) :  

* 如果上级文件运行在Node环境 , 子文件相对路径参考上级文件 . 
* 如果上级文件运行在浏览器环境 , 子文件相对路径参考应用根目录 , 例如配置文件的路径 . 