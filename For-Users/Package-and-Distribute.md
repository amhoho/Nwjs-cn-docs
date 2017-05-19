# 打包与发布

## 快速开始

可以使用以下工具自动完成打包NW.js应用进行发布 . 

* [nwjs-builder-phoenix](https://github.com/evshiron/nwjs-builder-phoenix) (推荐)
* [nw-builder](https://github.com/nwjs-community/nw-builder)

或者可以使用以下步骤手动构建应用 . 

## 准备应用

打包前 , 需要准备所有必要的文件 , 以下是需要检查的信息 , 不可缺少 . 

* [ ] 代码和资源文件
* [ ] NPM模块 , 可以执行`npm install`
* [ ] [应用中使用的本地模块](Advanced/Use-Native-Node-Modules.md)
* [ ] [NaCl](Advanced/Use-NaCl-in-NW.js.md)
* [ ] [已编译代码](Advanced/Protect-JavaScript-Source-Code.md)以及删除原始文件
* [ ] [配置文件](../References/Manifest-Format.md#icon)中使用的图标

!!! 警告
	nodejs模块在一个平台正常可用不代表所有平台运行都没问题 . 例如 `node-email-templates`能够在Windows和Mac系统通过 `npm install`命令进行安装 . 此外 , 需要python环境 , Windows系统默认不安装 . 
    
根据经验 , 为了确保不出问题 , 在想要运行应用的每个平台上安装相关的Nodejs模块 , 并进行打包 . 

!!! 注 "文件名和路径"
	在大多数Linux系统以及部分Mac OS X系统中 , 文件系统是大小写敏感的 . 既 `test.js`和 `Test.js`是不同的文件 . 确保路径以及文件名使用正确 . 

!!! 注 "Windows系统中的长路径"
	Windows系统中应用中使用的路径最大长度不能超过260个字符 . 如果过长 , 打包过程将会报错 . 该问题通常发生在使用低于3.0版本的NPM安装依赖包的过程 . 为了避免这个问题的发生 , 建议在根目录中构建引用 , 比如 `C:\build\` . 

## 准备NW.js

应用可以选择不同的NW.js构建方式 , NW.js提供多种[构建方式](Advanced/Build-Flavors.md)以便满足不同的需求以及应用大小 . 选择适当的构建方式或者[源码构建](../For-Developers/Building-NW.js.md)

应用重新分配NW.js包含的所有文件 , 除了SDK构建方式包含的 `nwjc`, `payload`以及 `chromedriver` . 


## 打包引用

两种打包方式: 普通文件和ZIP文件

### 方式 1. 普通文件 (推荐)

Windows和Linux系统 , 可以将应用的文件放到NW.js目录下 , 确保 `nw`(或者 `nw.exe`)与 `package.json`在同级目录下 . 或者将应用文件放入名为 `package.nw`目录中 , 该目录与 `nw`(或者 `nw.exe`)在同级目录下 . 需要注意 , `package.json`需要在 `package.nw`目录中 . 

Mac系统 , 将应用相关文件放入名为 `app.nw`中 , 同时将该文件添加到 `nwjs.app/Contents/Resources/`中 . 同样 , `package.json`需要在 `app.nw`中 . 

推荐使用普通文件打包方式 . 

### 方式 2. ZIP文件

你可以将应用相关文件打包到名为 `package.nw`的ZIP文件中 . Windows和Linux系统 , 将 `package`文件放入与 `nw`(或者 `nw.exe`)同级目录中 . Mac系统中 , 将 `package.nw`文件放入 `nwjs.app/Contents/Resources/`目录下 . 

!!! 警告 "包过大或包中文件数过多引起的开始加载慢"
	运行开始 , NW.js需要加压包内文件到临时目录 , 之后进行加载 . 包过大或包中文件过多会引起开始运行变慢 . 

Windows和Linux系统 , 可以将ZIP文件和 `nw`(或者 `nw.exe`)进行合并为一个文件

Windows系统运行命令如下:
```batch
copy /b nw.exe+package.nw app.exe
```
Linux系统运行命令如下:
```bash
cat nw app.nw > app && chmod +x app 
```

## 系统特性

### Windows

`nw.exe`图标替换工具 , 如[Resource Hacker](http://www.angusj.com/resourcehacker/) [nw-builder](https://github.com/mllrsohn/node-webkit-builder) [node-winresourcer](https://github.com/felicienfrancois/node-winresourcer).

你可以创建一个安装程序完成应用文件的安装 , 如[Windows Installer](https://msdn.microsoft.com/en-us/library/cc185688(VS.85).aspx) [NSIS](http://nsis.sourceforge.net/Main_Page) [Inno Setup](http://www.jrsoftware.org/isinfo.php).

### Linux

应用需要创建[`.desktop`文件](https://wiki.archlinux.org/index.php/Desktop_Entries).

创建自解压安装脚本 , 可以使用[`shar`](https://en.wikipedia.org/wiki/Shar) [`makeself`](http://stephanepeter.com/makeself/) . 

可以通过系统包管理工具发布应用 , 如 `apt`, `yum`, `pacman`等 , 请参考官方文档进行打包 . 

### Mac OS X

Mac OS X系统中 , 需要修改以下文件:

* `Contents/Resources/nw.icns`: 应用图标. `nw.icns`遵循[苹果图标图片格式](https://en.wikipedia.org/wiki/Apple_Icon_Image_format). 可以使用[Image2Icon](http://www.img2icnsapp.com/)将PNG/JPEG格式转换为ICNS格式图片 .
* `Contents/Info.plist`: 描述文件. 查看[关于面板的Cocoa标准](http://cocoadevcentral.com/articles/000071.php), 该文件将影响应用以及相关属性修改 .

应用需要进行签名 . 参考[应用签名及安装](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/DistributingApplicationsOutside/DistributingApplicationsOutside.html)              

## 参考

查看[wiki](https://github.com/nwjs/nw.js/wiki/How-to-package-and-distribute-your-apps)获取更多应用打包工具 .
