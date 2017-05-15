# 自定义菜单
> 不同平台的窗口菜单有不同的方式，本文展示不同系统中菜单的差异并提供了确保窗口菜单在所有平台上正常工作的方法。

## 创建并设置菜单栏

要创建菜单栏，您只需利用nw.Menu对象传入参数`type: 'menubar'`完成 :

```javascript
var your_menu = new nw.Menu({ type: 'menubar' });
```

还可以将子菜单添加到每个菜单项中 . 但大多数系统中 , 纯文本菜单是没什么意义的 . 

```javascript
var submenu = new nw.Menu();
// 向子菜单中添加菜单项 . 
submenu.append(new nw.MenuItem({ label: 'Item A' }));
submenu.append(new nw.MenuItem({ label: 'Item B' }));

// 子菜单添加到菜单栏
your_menu.append(new nw.MenuItem({
  label: 'First Menu',
  submenu: submenu
}));
```

然后，您可以通过设置窗口的 `meu`属性来设置窗口菜单:

```javascript
nw.Window.get().menu = your_menu;
```

更多谢姐,请参考[菜单](../../References/Menu.md)和[窗口](../../References/Window.md)章节

## 系统中菜单差异

### Windows & Linux

在Windows和Linux上，菜单栏的行为完全相同。 每个窗口都可以有一个菜单栏，它们都位于标题栏下方.

!!! "全屏 / Kiosk 模式"需知:

全屏/窗口模式 , 菜单栏将显示在窗口顶部 . 可以通过设置 `win.menu`为 `null`实现移除菜单条 . 参考[`win.menu`](../../References/Window.md#winmenu)

### Mac OS X

Mac上的NW.js应用只能有一个被称为应用程序菜单的菜单，无论应用程序有多少窗口。 许多快捷键都依赖于应用程序菜单的存在，如*Quit*, *Close* 和 *Copy*.

默认情况下， NW.js应用有一个默认的菜单及菜单项如*应用名*, *编辑*和*窗口*。 您可以使用[menu.createMacBuiltin](../../References/Menu.md#menucreatemacbuiltinappname-options-mac)方法获取默认菜单，并根据需要进行自定义:

```javascript
var mb = new nw.Menu({type:"menubar"});
mb.createMacBuiltin("your-app-name");
// 通过mb对象完成菜单的添加 , 插入以及删除
nw.Window.get().menu = mb;
```

!!! "修复应用程序菜单的标题"需知:
    应用程序菜单的第一项默认显示*nwjs*而非*your-app-name*. 要解决这个问题，您需要将 `nwjs.app/Contents/Resources/*.lproj/InfoPlist.strings`所有文件中的 `CFBundleName`设置的 `nwjs`换成 `your-app-name`.

## 最佳方法

如上所述，在Windows和Linux上，每个窗口都可以有一个菜单栏，而在Mac上，一个应用程序只能有一个应用程序菜单。

 所以通常只需在主窗口中设置菜单 , 同时,尽量避免在多个窗口中使用菜单 . 

不过,您可以使用[process.platform](http://nodejs.org/api/process.html#process_process_platform) 来获取平台并为不同平台设计不同菜单。