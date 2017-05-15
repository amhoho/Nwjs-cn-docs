# 专有编解码器

## 预制构建的NW.js支持专有编解码器

基于Chromium的NW.js , 媒体模块本质上是相同的 .

预制构建的NW.js支持以下编解码器:

```none
theora,vorbis,vp8,pcm_u8,pcm_s16le,pcm_s24le,pcm_f32le,pcm_s16be,pcm_s24be
```

同时支持demuxers:

```none
ogg,matroska,wav
```

## 在NW.js中启用专有编解码器

!!! "许可证和专利费"需知:
    使用MP3和H.264编解码器要求您注意专利使用费和源代码许可。如果在应用中需要使用专有媒体格式 , 可以咨询律师了解相关权限内容。有关源代码许可的更多信息，请查看[这里](https://chromium.googlesource.com/chromium/third_party/ffmpeg.git/+/master/CREDITS.chromium)    

!!! "警告"需知:
    如果您没有**没有**许可证,请根据下列提示或其它解决方案获得编解码器资格:

### 从社区获取FFmpeg二进制文件

您可以从社区获取[预编译FFmpeg二进制文件](https://github.com/iteufel/nwjs-ffmpeg-prebuilt/releases)或按下文自行构建FFmpeg:

### 脱离NW.js构建FFmpeg的DLL文件

如果您使用的是预制构建的NW.js， 只能够重新构建FFmpeg的DLL文件 , 替换预制构建NW.js中的DLL文件。 该过程需要下载大约1G大小文件以及个i编译约20G大小的NW.js文件 . 

 **第一步:** 在 [这里](https://github.com/nwjs/chromium.src/tags)下载Chromium定制包. 源文件位于解压后的 `~/<sub-directory-name>`目录.

 **第二步:** 因为您未构建整个NW.js, 所以您需要手动获取下列依赖:

* 从 `DEPS`拷贝以下目录文件:
    - `buildtools`
    - `tools/gyp`
    - `third_party/yasm/sources/patched-yasm`
    - `third_party/ffmpeg`
    
* 用下列命令在 `buildtools/<os>`中下载 `gn` 工具:
```bash
download_from_google_storage --no_resume \
                             --platform=<platform> \
                             --no_auth \
                             --bucket chromium-gn \
                             -s buildtools/<os>/<gn-exe>.sha1
```
    - `<platform>`: `win32` 表示 Windows; `darwin` 表示 Mac; `linux*` 表示 Linux
    - `<os>`: `win` 表示 Windows; `mac` 表示 Mac; `linux64` 表示 Linux
    - `<gn-exe>`: `gn.exe` 表示 Windows; `gn` 表示 Mac 和 Linux
    
* **[Mac and Linux]** 用下列命令在 `third_party/llvm-build`中下载 `clang`工具:
```bash
python tools/clang/scripts/update.py --if-needed
```

* **[Linux]** 用下列命令在 `build/linux/*-sysroot`中下载libraries:
```bash
python build/linux/sysroot_scripts/install-sysroot.py --running-as-hook
```

* **[Mac]** 用下列命令在 `third_party/libc++-static`中下载 **libc++-static**:
```bash
download_from_google_storage --no_resume \
                             --platform=darwin \
                             --no_auth \
                             --bucket chromium-libcpp \
                             -s third_party/libc++-static/libc++.a.sha1
```

!!! " Linux开发者"需知:
    首次构建FFmpeg的DDL文件或者执行下一步之前 , 请先运行 `build/install-build-deps.sh` 建立FFmpeg DLL 或 NW.js，此脚本将自动为您安装构建依赖项.

 **第三步:**  更换 BUILD.gn

在源代码的根目录中更换 `BUILD.gn`:

```
action("dummy") {
  deps = [
    "//third_party/ffmpeg"
  ]
  script = "dummy"
  outputs = ["$target_gen_dir/dummy.txt"]
}
```

 **第四步:**  利用GN生成 Ninja文件

```bash
cd path/to/nw/source/folder  
gn gen //out/nw \
 --args='is_debug=false is_component_ffmpeg=true target_cpu="<target_cpu>" is_official_build=true ffmpeg_branding="Chrome"'
```

 **注意**: `<target_cpu>` 应设置 `x86` 或 `x64`

 **第五步:** 构建 ffmpeg DLL文件

```bash
ninja -C out/nw ffmpeg
```

您可以在 `out/nw` 中找到DLL. 路径和文件名称因平台而异:

* Windows: `ffmpeg.dll`
* Mac OS X: `libffmpeg.dylib`
* Linux: `lib/libffmpeg.so`

 **第六步:** 用第五步的DLL文件替换NW.js中的DLL. 路径和文件名称因平台而异:

* Windows: `ffmpeg.dll`
* Mac OS X: `nwjs.app/Contents/Versions/<chromium-version>/nwjs Framework.framework/libffmpeg.dylib`
* Linux `lib/libffmpeg.so`

### 使用专有编解码器构建整个NW.js

如果您不使用官方预制的NW.js，则可以按照以下说明构建启用了专有编解码器的整个NW.js。有关每个步骤的详细信息，请参阅[构建NW.js](Building NW.js.md).  

 **第1步:** 构建前提并获取 NW.js 源代码. 详细参考[构建NW.js](Building NW.js.md)文件的 *构建前提* 和 *获取代码* 章节

 **第2步:** 配置GN时，将 `ffmpeg_branding`设置为 `Chrome`.

 **第3步:** 再次重新生成 ninja 文件.

 **第4步:** 重建 NW.js.
