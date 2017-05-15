# DOM变化

## &lt;input type="file"&gt;

HTML5支持文件对话框标签 `<input type="file" />` , 并提供了属性如[`multiple`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#attr-multiple) , [`accept`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#attr-accept)以及 `webkitdirectory` 等 .

NW.js应用更好的扩展了文件输入框 . 但考虑到安全性 , NW.js扩展的属性只能在Node框架中启用 . 参考[安全](../For-Users/Advanced/Security-in-NW.js.md)查看Node和普通框架的区别 . 

### fileinput.value
> 用途:**包含本地文件的原路径**
 
例如,您可以使用Node.js API读取用户选择的文件路径:

```javascript
// 获取选择文件的原路径
var fileinput = document.querySelector('input[type=file]');
var path = fileinput.value;

// 使用Node.js的API读取文件
var fs = nw.require('fs');
fs.readFile(path, 'utf8', function(err, txt) {
  if (err) {
    console.error(err);
    return;
  }

  console.log(txt);
});
```

### fileitem.path
> 用途:**获取 `files`中选择的每个文件原路径**
 
HTML5提供了 `files`属性来返回在 `<input>`标签中选择的所有文件。

NW.js则为 `files`提供了一个额外属性 `fileitem.path`获取 `files`中选择的每个文件原路径,

```javascript
var fileinput = document.querySelector('input[type=file]');
var files = fileinput.files;

for (var i = 0; i < files.length; ++i) {
  console.log(files[i].path);
}
```

### 标签属性: `nwdirectory`
> 属性:**`nwdirectory`属性类似于 `webkitdirectory` , 但返回值为路径 , 而不是文件对象**
 
```html
<input type="file" nwdirectory>
```

### 标签属性: `nwsaveas`
> 属性:**`nwsaveas`属性可打开让可输入文件路径的 `'另存为'`对话框.  该方式不同于默认的文件选择标签**

```html
<input type="file" nwsaveas>
```

该属性还支持指定默认的存储文件名称:

```html
<input type="file" nwsaveas="filename.txt">
```

### 标签属性: `nwworkingdir`
> 属性:**当激活元素时， `nwworkingdir`属性可令文件对话框将在给定目录中打开**

例如，在 `/home/path/`中打开文件对话框:

```html
<input type="file" nwworkingdir="/home/path/">
```
### 事件: `oncancel` 
> 触发:**取消对话框时**

## &lt;iframe&gt;

NW.js扩展了 `<iframe>`标签使应用开发更加简单 , 可以绕过沙箱限制以及同源策略等跨域问题 . 

参考[webview标签](webview-Tag.md)中的  `<webview>`标签 . 

### 标签属性: nwdisable
> 属性:**使框架和自框架为正常框架**

注意,该属性不能阻止页面中正常框架访问父页面以及体层框架 . 但仍然能够访问Node.js的API . 该属性通常与 `nwfaketop`属性一同使用 . 

### 标签属性: nwfaketop
> 属性:**阻止框架中页面访问 `window.parent`或 `window.top`**
 
框架自己拥有 `window`对象 , 其子框架也会受到影响

该属性通常与 `nwdisable`一同使用

### 标签属性: nwUserAgent
> 属性:**框架和自框架加载页面时重写 `user-agent`属性**

更多细节,请参考[`user-agent`配置](Manifest-Format.md#user-agent)   