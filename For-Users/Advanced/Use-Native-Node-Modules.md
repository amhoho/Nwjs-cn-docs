# 在NW.js中安装Node原生模块

## 使用NPM安装

### 对于LTS版本

如果您使用的是LTS版本，在 Windows, 在使用 `node-gyp` 或 `npm`安装原生模块之前,您需要用[这个文件](https://github.com/nwjs/nw.js/blob/nw18/tools/win_delay_load_hook.cc) 替换系统中的 `<npm-path>\node_modules\node-gyp\src\win_delay_load_hook.cc`,仅在您使用相同版本和架构的Node.js和NW.js.s才有效.

### 对于非LTS版本

由于[V8中的ABI差异](https://github.com/nwjs/nw.js/issues/5025),`nw-gyp`需要全局预安装才能构建模块

要为NW.js安装原生模块，请在终端中运行以下命令：

```bash
＃全局安装nw-gyp
npm install -g nw-gyp
＃设置目标NW.js版本
export npm_config_target=0.18.5
＃设置构建架构，ia32或x64
export npm_config_arch=x64
export npm_config_target_arch=x64
＃使用node-pre-gyp构建的模块的设置env
export npm_config_runtime=node-webkit
export npm_config_build_from_source=true
＃将nw-gyp设置为node-gyp
export npm_config_node_gyp=$(which nw-gyp)
＃运行npm安装
npm install
```

!!! "Windows系统"需知:
* 安装编译器. 比如 [Visual C++构建工具](http://landinghub.visualstudio.com/visual-cpp-build-tools).             
* Python 2.7(不支持3.x版本).
* 将 `npm_config_node_gyp`环境变量必须设置为 `path\to\global\node_modules\nw-gyp\bin\nw-gyp.js`而非 `nw-gyp.cmd` 处理脚本. 
* 在Windows中设置环境变量时必须 `set` 而非 `export`.

一个例子(Windows 10, Visual C++ Build Tool v2015): 

```Batchfile
set PYTHON=C:\Python27\python.exe
set npm_config_target=0.21.6
set npm_config_arch=x64
set npm_config_runtime=node-webkit
set npm_config_build_from_source=true
set npm_config_node_gyp=C:\Users\xxxxxxxxx\AppData\Roaming\npm\node_modules\nw-gyp\bin\nw-gyp.js
npm install --msvs_version=2015
```

## 手动重建

!!!"完整安装npm"需知:
    当切换NW.js版本后, 推荐做法是删除 `node_modules`目录并安装[这篇文档](#install-with-npm)完整安装npm  

一旦切换新版本,您可以使用下面这些工具重建 **每个原生模块**而 **无需重新安装所有模块**. 

由于 `binding.gyp` 是构建原生模块所必需的, 所以您可以查找 `binding.gyp` 文档来找到所有的原生模块.

!!! "重建所有原生模块"需知:
进行重建时,可能会遇到各种问题甚至崩溃,所以一旦手动重建不成功,您可以尝试完整的npm安装

### nw-gyp

[`nw-gyp`](https://github.com/nwjs/nw-gyp)源自 `node-gyp`,支持NW.js特定的头文件和库.其功能与node-gyp工具相同 , 除了需要手动指定版本以及arch( `x64`或 `ia32`)

```bash
npm install -g nw-gyp
cd myaddon
nw-gyp rebuild --target=0.13.0 --arch=x64
```

详细内容请参考[这里](https://github.com/nwjs/nw-gyp)    

### node-pre-gyp

部分包使用了[node-pre-gyp](https://github.com/mapbox/node-pre-gyp), 它支持通过 `node-gyp` 或 `nw-gyp`来构建 Node.js 和 NW.js。

 `node-pre-gyp`的用法如下:
 
```bash
npm install -g node-pre-gyp
cd myaddon
node-pre-gyp build --runtime=node-webkit --target=0.13.0 --target_arch=x64
```

详细内容请参考[这里](https://github.com/mapbox/node-pre-gyp)