# Chrome扩展API
> NW.js支持[Chrome应用平台](https://developer.chrome.com/apps/api_index)中所有chrome.*的API

此外 , NW.js支持[扩展平台](https://developer.chrome.com/extensions/api_index)中部分chrome.*的API . 可参考文档:

* [chrome.contentSettings](https://developer.chrome.com/extensions/contentSettings) : 控制通知设置
* [chrome.tabs](https://developer.chrome.com/extensions/tabs) : 支持开发工具扩展 . 
* [chrome.proxy](https://developer.chrome.com/extensions/proxy) : 代理设置管理 . 
* [chrome.cookies](https://developer.chrome.com/extensions/cookies) :  `get`, `getAll`, `set` 和 `remove` 这些方法已被支持于带有storeId webview中(`webview.getCookieStoreId()`)