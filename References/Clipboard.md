# 剪贴板
> `Clipboard`是Windows, Linux 和 Mac系统中的常用操作.

## 示例

```javascript
//获取系统剪贴板
var clipboard = nw.Clipboard.get();

//从剪贴板中读取
var text = clipboard.get('text');
console.log(text);

//或写点东西到剪贴板
clipboard.set('我在使用NW.js :)', 'text');

// 或者清空剪贴板
clipboard.clear();
```

## Clipboard.get()
> 用途:**获取剪贴板对象 `Clipboard`**

 请注意,X11不支持选择剪贴板 . 

## clip.set(data, [type, [raw]])
> 用途:**将对应类型的数据 `data`写入剪贴板**

* `data` String - 要写入剪贴板的数据
* `type` String - (可选) 数据类型,可选 `text`, `png`, `jpeg`, `html` 和 `rtf`,默认类型为 `"text"`.
* `raw`  Boolean - (可选)  是否需要原始图片数据. 此选项仅在类型为 `png` 或 `jpeg`有效. 默认为 `false`.

该方法将清空剪贴板内容以及将 `data`内容写入剪贴板 . 再次调用该方法会将新 `data`内容替换到现 有`data`内容中 . 

如需同时写入多种类型的数据到剪贴板,请使用[clip.set(clipboardDataList)](clipsetclipboardDataList).

请注意,读取或写入的图像仅为 `JPEG`或 `PNG`.

当未设置 `raw`或设为 `false`时, `data`参数为有效的Base64编译的[URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/data_URIs). 

当设置 `raw`设为 `true`时,`data`参数为Base64编译的图片数并且不含 `data:<mime-type> ;base64,`部分 . 


## clip.set(clipboardData)
> 用途:**将剪贴板数据的JSON对象 `clipboardData`写入剪贴板**

* `clipboardData` Object - 要写入剪贴板的内含 `data`, `type` 和 `raw`的JSON对象. 详情请参阅 [clip.set(data, [type, [raw]])](#clipsetdata-type-raw)章节

## clip.set(clipboardDataList)
> 用途:**批量将剪贴板数据的JSON对象组成的数组 `clipboardData`写入剪贴板**

* `clipboardDataList` Array -  要写入剪贴板的多个内含 `data`, `type` 和 `raw`的JSON对象组成的数组数据. 请参阅 [clip.set(clipboardData)](#clipsetclipboardData) 和 [clip.set(data, [type, [raw]])](#clipsetdata-type-raw)章节

可以使用此方法同时将多种类型的数据写入剪贴板.

例如: 将图像和指向图像的 `<img> ` 写入剪贴板:

```javascript
var fs = require('fs');
var path = require('path');

//使用绝对路径以便其他应用能够使用
var pngPath = path.resolve('nw.png');
//读取图片文件并通过base64进行编码
var data = fs.readFileSync(pngPath).toString('base64');
//将文件路径转换为URL
var html = '<img src="file:///' + encodeURI(data.replace(/^\//, '')) + '"> ';

var clip = nw.Clipboard.get();
// 将PNG和HTML写入剪贴板
clip.set([
  {type: 'png', data: data, raw: true},
  {type: 'html', data: html}
]);
```

## clip.get([type, [raw]])
> 用途:**从剪贴板获取指定类型 `type`的数据( `String`)**

* `type` String - (可选)  数据的类型,可选 `text`, `png`, `jpeg`, `html` and `rtf`. 默认为 `"text"`.
* `raw`  Boolean - (可选)  是否需要原始图片数据. 此选项仅在类型为 `png` 或 `jpeg`有效. 默认为 `false`.

## clip.get(clipboardData)
> 用途:**从剪贴板获取内含 `data`, `type` 和 `raw`的JSON对象的数据( `clipboardData`)**

* `clipboardData` Object - 从剪贴板中读取的内含 `data`, `type` 和 `raw`的JSON对象. 详情请参阅  [clip.get([type, [raw]])](#clipgettype-raw)章节
* 返回 String - 返回剪贴板中数据 . 

## clip.get(clipboardDataList)
> 用途:**从剪贴板获取多个内含 `data`, `type` 和 `raw`的JSON对象组成的数组数据( `Array`)**

* `clipboardDataList` Array -  从剪贴板中读取的多个内含 `data`, `type` 和 `raw`的JSON对象组成的数组数据. 详情请参阅 [clip.get(clipboardData)](#clipgetclipboardData) 和 [clip.get([type, [raw]])](#clipgettype-raw)章节
* 返回 Array -  从剪贴板读取的 `clipboardData`数组,  . 每个元素内容包括`type`, `data`和 `raw`(可选)参数.

可以使用此方法同时从剪贴板读取多种类型的数据.

## clip.readAvailableTypes()
> 用途:**返回由当前系统支持的剪贴板类型组成的数组( `Array`)**

    - `text`: 文本,可使用 `clip.get('text')`读取
    - `html`: HTML文本,可使用 `clip.get('html')`读取
    - `rtf`: 富文本,可使用 `clip.get('rtf')`读取
    - `png`: PNG图像,可使用 `clip.get('png')`读取
    - `jpeg`: JPEG图像,可使用 `clip.get('jpeg')`读取

## clip.clear()
> 用途:**清空剪贴板**