# 系统托盘
> `Tray`通常是操作系统显示小图标的区域MAC上称为 `状态项`, GTK为 `状态图标`, Windows称为 `系统托盘图标`.

## 实例

```javascript
// 创建托盘图标
var tray = new nw.Tray({ title: 'Tray', icon: 'img/icon.png' });

// 创建托盘菜单
var menu = new nw.Menu();
menu.append(new nw.MenuItem({ type: 'checkbox', label: 'box1' }));
tray.menu = menu;

// 移除托盘图标
tray.remove();
tray = null;
```

由于页面中创建的托盘在关闭窗口或导航后会被当成垃圾回收而消失不见,因此,请在 **后台页面**中使用托盘,该页面存在于应用的整个生命周期中.关于如何在后台页面中执行脚本,请参考[`bg-script`](Manifest-Format.md#bg-script) 和 [`main`](Manifest-Format.md#main)章节.

## new Tray(option)
> 用途:**创建新的系统托盘**

* `option` Object -
    - `title` String - 标题
    - `tooltip` String - 悬停提示内容
    - `icon` String - 图标
    - `alticon` String - 备用图标
    - `iconsAreTemplates` Boolean - 是否模板图标 默认 `true`
    - `menu` Menu - 弹出的菜单



## tray.title
> 用途:**设置或获取 `Tray`对象中设置的 `title`标题**

Mac中标题与图标一起显示在托盘中,但GTK 和 Windows都仅显示图标.

## tray.tooltip
> 用途:**设置或获取 `Tray`对象中设置的 `tooltip`悬停文本提示**
 
 `tooltip`即鼠标悬停在图标时希望显示的内容提示,它是 `Tray`中的一个String属性.

## tray.icon
> 用途:**设置或获取 `Tray`对象中设置的 `icon`图标**

  `icon`即图标文件路径,可以指向应用内置图标的相对路径,也可以是系统中的文件的绝对路径

请注意,在Mac中,png图标并非Windows通知区域那样大小,而是1:1的实际大小.

## tray.alticon (Mac)
> 用途:**设置或获取 `Tray`对象中设置的 `alticon`备用图标**

## tray.iconsAreTemplates (Mac)
> 用途:**设置或获取 `Tray`对象中设置的  `iconsAreTemplates`是否模板图标, 默认 `true`**

设置为 `true`时, `icon` 将被视为 `模板图像`,可配合系统状态(如灰暗/明亮模式)等自动确保所需样式.板图像应该仅由黑色和清晰的颜色组成，并且可以使用图像中的Alpha通道来调整黑色内容的不透明度。

## tray.menu
> 用途:**设置或获取 `Tray`对象中设置的 `menu`弹出的菜单**

 `menu`菜单将在您右击(MAC中点击)图标时显现.

## tray.remove()
> 用途:**移除托盘**

一旦移除,则无法再次显示.您可以使用Tray(null)使其被垃圾回收.目前实现无法暂时性的隐藏图标.

## 事件: click
> 触发:**用户点击图标时,该事件无法用于右击或者双击事件**

请注意,在Mac由于此类行为将导致NW.js应用程序无法发布至MAS(应用商店)中,所以NW.js不支持menulet (<kbd>&#8984;</kbd>+drag)