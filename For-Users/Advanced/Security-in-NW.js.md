# NW.js安全性

## Node frame和标准frame
NW.js有两种frame , 分别是Node frame和标准frame

 **Node frame**比 **标准frame**多以下功能:

* 可以访问Node.js的APIs以及NW.js的APIs
* 可以继承DOM特性 , 例如[存储会话](../../References/Changes-to-DOM.md#attribute-nwsaveas) , [nwUserAgent属性](../../References/Changes-to-DOM.md#attribute-nwuseragent)等 . 
* 旁路安全限制 , 比如沙箱 , 同源策略等 . 例如 , 你可以跨域访问任何一个站点 , 或者在Node frame中 , 访问 `<iframe>`属性 `src`指向的站点 . 

NW.js中 , **node frame**遵循以下标准:

* [配置文件](../../References/Manifest-Format.md#nodejs)中的`nodejs`设置为true
* [配置文件](../../References/Manifest-Format.md#node-remote)中 , URL和frame匹配 `node-remote`模式或者 `chrome-extension://`协议
* Frames和父类frames没有[`nwdisable`属性](../../References/Changes-to-DOM.md#attribute-nwdisable)
* Frames和父类frames不在[`<webview>`标签](../../References/webview-Tag.md)中
