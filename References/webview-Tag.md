# &lt;webview&gt; 标签

 `webview` 标签可将 `guest` 内容（例如外部网页）嵌入到应用中. 它不像 `iframe` , `webview` 和你的应用运行的是不同的进程. 

它不具有与您的网页相同的权限，为确保应用不受嵌入内容的影响,应用和嵌入内容之间的交互全部都是异步的.

## 使用例子

 `webview`嵌入范例:

```html
<webview id="foo" src="http://www.google.com/" style="width:640px; height:480px"></webview>
```

## 参考文献

更多细节,请参考 [Chrome `<webview>`标签](https://developer.chrome.com/apps/tags/webview) 列举的更详细的API.

除了上游API之外，NW.js还添加了以下方法：

### webview.showDevTools(show, [container])

* `show` Boolean - 打开或关闭调试工具
* `container`  webview Element -(可选) 用于显示调试工具中的 `<webview>`标签. 默认在新窗口中打开调试工具

### 在webview中加载本地文件

将以下内容权限添加到配置文件中：
```json
  "webview": {
     "partitions": [
        {
          "name": "trusted",
          "accessible_resources": [ "<all_urls>" ]
        }
     ]
  }
```

并在webview标签中设置 `partition="trusted"`属性.

### 在webview中启用Node.js

在webview中启用Node.js,需在webview标签中设置 `allownw`属性.

该方法在本地和远程站点中都应当谨慎使用,因为webview通常用于加载不信任的内容.

### 在webview中启用Cookies

webview中 `getCookieStoreId()`函数进行返回可以在[chrome.cookies]（https://developer.chrome.com/extensions/cookies）API中使用的storeId。

##### 控制台中的示例:

```html
chrome.cookies.getAll({url:"http://docs.nwjs.io", storeId:webview.getCookieStoreId()}, console.log.bind(console));
```