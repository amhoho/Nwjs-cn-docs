# 在NW.js中使用Flash插件

NW.js支持Pepper Flash插件 . 在NW.js应用nw同级目录中将插件(包括配置文件 `'manifest.json'`)需要放入一个名为 `'PepperFlash'`的子目录.

Mac系统中 , 该子目录还需要放入`"Internet Plug-Ins"`目录中 . 

可用 `/path/to/nwjs --url='chrome://plugins'`验证flash插件加载过程 . 