# 保护JavaScript源代码
> 应用中JavaScript源代码能够编译为本地代码进行保护 , NW.js能够加载编译之后的代码 . 应用作为产品发布时可以将代码进行编译 . 

### 编译

JS源代码编译为本地代码需要使用 `nwjc`工具 , 同时需要提供SDK构造方式的NW . 

```bash
nwjc source.js binary.bin
```

`*.bin`文件需要发布到应用中 , 可以任意命名bin文件 . 

### 加载已编译JS文件

```javascript
nw.Window.get().evalNWBin(frame, 'binary.bin');
```
[win.evalNWBin()](../../References/Window.md#winevalnwbin)方法中的参数与 `Window.eval()`方法相同 , 第一个参数为目标frame , `null`为主frame , 第二个参数为已编译的bin文件 . 

### 从远程加载已编译JS文件
可以从远程（例如使用AJAX）获取已编译的JavaScript，并且即时执行。

```javascript
var xhr = new XMLHttpRequest();
xhr.responseType = 'arraybuffer'; // make response as ArrayBuffer
xhr.open('GET', url, true);
xhr.send();
xhr.onload = () => {
  // xhr.response contains compiled JavaScript as ArrayBuffer
  nw.Window.get().evalNWBin(null, xhr.response);
}
```

	已编译代码在[浏览器环境](JavaScript-Contexts-in-NW.js.md#browser-context)中执行 . 可以像其他运行在浏览器环境中的其他脚本 , 就像您使用任何Web API(如DOM)以及[NW.js API和Node API](JavaScript-Contexts-in-NW.js.md#access-nodejs-and-nwjs-api-in-browser-context) . 

### 已知问题

已编译代码不支持跨平台也不兼容不同版本的NW.js的版本,因此在打包应用时需要在各自系统平台中运行`nwjc` . 