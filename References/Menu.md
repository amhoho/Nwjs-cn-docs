# 菜单
`Menu` 表示菜单栏或上下文菜单.  由[`MenuItem`](MenuItem.md) 菜单项组成. 

## 示例

创建上下文菜单的例子:
```javascript
// 创建一个空的上下文菜单
var menu = new nw.Menu();

// 添加一些项目
menu.append(new nw.MenuItem({ label: '菜单项 A' }));
menu.append(new nw.MenuItem({ label: '菜单项 B' }));
menu.append(new nw.MenuItem({ type: 'separator' }));
menu.append(new nw.MenuItem({ label: '菜单项 C' }));

// 删除一个项目
menu.removeAt(1);

// 弹出窗口作为上下文菜单
menu.popup(10, 10);

// 迭代菜单项
for (var i = 0; i < menu.items.length; ++i) {
  console.log(menu.items[i]);
}
```

想要创建菜单栏,您必须创建一个2级菜单并将其分配给`win.menu`.以下是创建菜单栏的示例:
```javascript
// 创建一个空的菜单栏
var menu = new nw.Menu({type: 'menubar'});

// 创建一个子菜单作为第二级菜单
var submenu = new nw.Menu();
submenu.append(new nw.MenuItem({ label: '菜单项 A' }));
submenu.append(new nw.MenuItem({ label: '菜单项 B' }));

// 创建并附加一级菜单到菜单栏
menu.append(new nw.MenuItem({
  label: '第一个菜单',
  submenu: submenu
}));

// 分配至`window.menu`并显示菜单
nw.Window.get().menu = menu;
```

更多细节,请参阅 [自定义菜单栏](../For-Users/Advanced/Customize-Menubar.md)          

由于页面中创建的菜单(如右键菜单)在关闭窗口或导航后会被当成垃圾回收而消失不见,因此,请在 **后台页面**中使用菜单,该页面存在于应用的整个生命周期中.关于如何在后台页面中执行脚本,请参考[`bg-script`](Manifest-Format.md#bg-script) 和 [`main`](Manifest-Format.md#main)章节.

## new Menu([option])
> 用途:**创建新的菜单**

* `option` Object - (可选) 
    - `type` String - (可选)  可选类型: `menubar`, `contextmenu` ,默认 `contextmenu`

## menu.items
> 用途:**获取由全部菜单项组成的数组**

 有关 `MenuItem`菜单项内容,请参阅 [MenuItem](MenuItem.md)

## menu.append(item)
> 用途:**在菜单尾部追加新的菜单项 `item`**

* `item`  MenuItem - 追加到菜单尾部的菜单项

## menu.insert(item, i)
> 用途:**在菜单指定索引位置插入新的菜单项 `item`**

* `item`  MenuItem - 要插入的菜单项
* `i` Integer - 在菜单中的位置索引,索引以 `0`开始

## menu.remove(item)
> 用途:**从菜单中移除指定 `item`的菜单项**

* `item`  MenuItem - 要移除的菜单项

## menu.removeAt(i)
> 用途:**从菜单中移除指定索引 `i`的菜单项 `item`**

* `i` Integer - 要移除的菜单项的位置索引

## menu.popup(x, y)
> 用途:**在指定锚点(x,y)弹出 `contextmenu`类型的上下文菜单**

* `x` Integer - 锚点的x位置
* `y` Integer - 锚点的y位置

通常,你要监听 `contextmenu` 并手动弹出菜单:

```javascript
document.body.addEventListener('contextmenu', function(ev) { 
  ev.preventDefault();
  menu.popup(ev.x, ev.y);
  return false;
});
```

这个方法可以精确的按需显示不同样式的菜单,而且弹出前还可以对菜单进行更新.

## menu.createMacBuiltin(appname, [options]) _Mac_
> 用途:**在Mac上的菜单栏中创建内置菜单如(*App*, *Edit* 或 *Window*)**

* `appname` String - 应用名,该名称将显示为 *App*菜单标题
* `options` Object - (可选) 
    - `hideEdit` Boolean - (可选)  不填充编辑菜单
    - `hideWindow` Boolean - (可选)  d不填充窗口菜单
    
更多细节,请参阅 [自定义菜单栏](../For-Users/Advanced/Customize-Menubar.md)          