# Shell桌面集成
>`Shell`是完成桌面相关工作的API集合

## 示例

```javascript
// 默认浏览器中打开指定URL
nw.Shell.openExternal('https://github.com/nwjs/nw.js');

// 默认编辑器中打开指定文件 . 
nw.Shell.openItem('test.txt');

// 文件管理器中显示指定文件所在目录 . 
nw.Shell.showItemInFolder('test.txt');
```

## Shell.openExternal(uri)
> 用途:**系统默认方式打开外部URI,如使用默认邮件方式打开mailto: URLs**

* `uri` String -系统默认方式打开URI

## Shell.openItem(file_path)
> 用途:**系统默认方式打开 `file_path`指定文件**

* `file_path` String - 本地文件路径

## Shell.showItemInFolder(file_path)
> 用途:**文件管理器打开`file_path`所在目录 , 同时选中文件**

* `file_path` String - 本地文件路径