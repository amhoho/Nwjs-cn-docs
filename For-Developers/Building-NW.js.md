# 构建NW.js

## 构建前提

NW.js的构建工具与步骤类似Chromium. 请根据平台阅读并安装 `depot_tools`和其它前提:

* [Windows](https://chromium.googlesource.com/chromium/src/+/master/docs/windows_build_instructions.md)
* [Mac OS X](https://chromium.googlesource.com/chromium/src/+/master/docs/mac_build_instructions.md)
* [Linux](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md)

!!! "Windows"需知:
    您需要运行 `set DEPOT_TOOLS_WIN_TOOLCHAIN=0` 或在环境中设置全局变量

!!! "Xcode 7"需知:
     不支持Xcode 7中的SDK 10.11 . 如果您已经升级到Xcode 7，则按照[Chromium文档](https://chromium.googlesource.com/chromium/src/+/master/docs/mac_build_instructions.md)降级到Xcode 6或拷贝其他设备的Mac SDK 10.10目录: ```xcode-select -p`/Platforms/MacOSX.platform/Developer/SDKs``

## 获取代码
 **第一步:** 创建用于存放Nw.js源码的目录如 `$HOME/nwjs`, 再使用下列命令生成 `.gclient` 文档:
 
```bash
mkdir -p $HOME/nwjs
cd $HOME/nwjs
gclient config --name=src https://github.com/nwjs/chromium.src.git@origin/nw17
```

您要是对运行Chromium测试没有兴趣,大可省略同步测试用例与参考构建,这样可以节省很多时间. 打开你刚生成的 `.gclient` 文档并用下文替换 `custom_deps`部分:

```python
"custom_deps" : {
    "src/third_party/WebKit/LayoutTests": None,
    "src/chrome_frame/tools/test/reference_build/chrome": None,
    "src/chrome_frame/tools/test/reference_build/chrome_win": None,
    "src/chrome/tools/test/reference_build/chrome": None,
    "src/chrome/tools/test/reference_build/chrome_linux": None,
    "src/chrome/tools/test/reference_build/chrome_mac": None,
    "src/chrome/tools/test/reference_build/chrome_win": None,
}
```

手动复制以及检查以下资源分支:

| path | repo |
|:---- |:---- |
| src/content/nw | https://github.com/nwjs/nw.js |
| src/third_party/node-nw | https://github.com/nwjs/node |
| src/v8 | https://github.com/nwjs/v8 |

 **第二步:** 终端中运行以下命令:
 
```
gclient sync --with_branch_heads
```

该命令将会从GitHub和Google下载超过20G的内容,所以请确保网络通畅. 一旦完成,您将看到一个 `.gclient`所在目录内将生成 `src`文件夹.


!!!  "Linux中首次构建时"需知:
    您必须运行 `gclient sync --with_branch_heads --nohooks` 和 `./build/install-build-deps.sh` 来安装Ubuntu相关依赖. 在 [Chromium 文档中](http://dev.chromium.org/developers/how-tos/get-the-code)有详细说明.

## 利用 GN 为 Chromium生成ninja构建文件

```bash
cd src
gn gen out/nw  (切勿更改 `nw`部分的路径,如果新建也不要改变 `out`部分) 
```

官方建议的构建参数:   
```bash
is_debug=false
is_component_ffmpeg=true
target_cpu="x64"
```

注意,`is_component_build = true` 即支持组件构建可缩短开发周期.有关GN 和 GYP 请参考[谷歌文档](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/docs/cookbook.md#Variable-mappings)        

## 利用 GYP 为 Node生成ninja构建文件

```bash
cd src
GYP_CHROMIUM_NO_ACTION=0 ./build/gyp_chromium -I third_party/node-nw/common.gypi third_party/node-nw/node.gyp
```

或者使用以下命令完成组件构建:

```bash
./build/gyp_chromium -D component=shared_library -I third_party/node-nw/common.gypi third_party/node-nw/node.gyp
```

如需更改Node的构建配置，则需要设置GYP_DEFINES环境变量:

### 32-bit/64-bit 构建方法

* Windows
    - 32-bit: 默认构建
    - 64-bit: `set GYP_DEFINES="target_arch=x64"` 并在 `out/Debug_x64` 或 `out/Release_x64`中重新构建
* Linux
    - 32-bit: **TODO: chroot**
    - 64-bit: 默认构建
* Mac
    - 32-bit: `export GYP_DEFINES="host_arch=ia32 target_arch=ia32"` 并在 `out/Debug` 或 `out/Release` 中重新构建
    - 64-bit: 默认构建

## 构建nwjs

运行GN后, `out/nw`中将生成ninja构建文件. 运行以下命令则会在 `out/nw` 文件夹中生成标准NW.js二进制代码:

```bash
cd src
ninja -C out/nw nwjs
```

!!! "构建耗时"需知:
    取决于您的机器性能,完全构建可能耗时几个小时,推荐的配置是多喝CPU>=8核,硬盘>SSD,内存>8G的PC.阅读[快速构建](#build-faster)章节可加快一些速度.

构建32/64位或者非标准构建方式 , 可以通过编辑 `out/nw/args.gn`文档并重新运行上述命令以生成二进制文件。

## 构建Node

```bash
cd src
ninja -C out/Release node
```

构建Node之后 , 最后需要拷贝Node构造的二进制文件到nwjs目录中:

```bash
cd src
ninja -C out/nw copy_node
```

## 构造方式

* Standard: `nwjs_sdk=false`
* SDK: `enable_nacl=true`

有关不同构造方式之间的区别，请参阅[对于用户目录中的构造方式](../For-Users/Advanced/Build-Flavors.md)章节                      

## 专有编解码器

由于许可证问题，NW.js的预构建二进制文件不支持如H.264等专有编解码器。因此你无法使用 `<audio>` 和 `<video>`标签播放mp3/mp4文档. 
要使用这些媒体，您必须通过按照[专有编解码器](Enable-Proprietary-Codecs.md)章节从源代码中构建NW.js   

## 快速构建

下列文档有助于加快您的构建:

* [Mac系统构建说明:快速构建](https://chromium.googlesource.com/chromium/src/+/master/docs/mac_build_instructions.md#Faster-builds)
* [Linux系统中快速构建方法](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_faster_builds.md)
