# 常见问题

## Linux系统终端中不能使用console.log打印信息
在命令行设置 `--enable-logging=stderr`参数 , 更多信息参考[chromium文档](https://www.chromium.org/for-testers/enable-logging)    

## `var crypto = require('crypto')`获取对象错误
Chromium有个名为 `crypto`的全局对象 , 该对象无法被重写 . 所以不要使用 `crypto`作为变量名 , 更改成其它如 `nodeCrypto`便可以使用 . 

## 在AnugarJS中 , 图片被打断并报错`Failed to load resource XXX net::ERR_UNKNOWN_URL_SCHEME`

AngularJS为了防止XSS攻击增加 `unsafe`前缀 . NW.js中的URLs和Chrome应用使用 `chrome-extension`作为协议头开始 , 因此不能识别AnuglarJS . 

解决方式为将AnuglarJS协议头添加到NW.js白名单配置中 , 方式如下:
```javascript
myApp.config(['$compileProvider',
  function($compileProvider) {
    $compileProvider.imgSrcSanitizationWhitelist(/^\s*((https?|ftp|file|blob|chrome-extension):|data:image\/)/);
    $compileProvider.aHrefSanitizationWhitelist(/^\s*(https?|ftp|mailto|tel|file|chrome-extension):/);
  }]);
```

## AngularJS 2中不能打印异常信息
AngularJS 2中试图创建名为 `global`的全局对象作为异常处理器 . 然而 , 名为 `global`全局对象已存在于NW.js环境中 , 故不能正常打印异常信息 . 

解决方法是重命名NW.js中的`global`全局对象给AngularJS使用 . 方式如下:
```html
<script>
window.nw_global = window.global;
window.global = undefined;
</script>
<!-- Angular 2 Dependencies -->
```

## 如何使用ESC键退出全屏模式
用户通常使用ESC键退出全屏模式 . NW.js默认不支持ESC快捷键退出全屏模式 , 但提供了更完整的全屏和退出全屏的API

开启ESC键退出全屏模式 , 请参考[快捷键](../References/Shortcut.md)章节:  
```javascript
nw.App.registerGlobalHotKey(new nw.Shortcut({
  key: "Escape",
  active: function () {
    // ESC键处理事件 , 退出全屏模式
    nw.Window.get().leaveFullscreen();
  }
}));
```