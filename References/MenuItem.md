# 菜单项
> `MenuItem`作为菜单项,可由分隔符或正常项组成,其中正常项通常包括了图文标签,以响应鼠标或键盘操作.

## 示例

```javascript
var item;

// 创建分隔符
item = new nw.MenuItem({ type: 'separator' });

// 创建具有图文的正常项
item = new nw.MenuItem({
  type: "normal", 
  label: "菜单项",
  icon: "img/icon.png"
});

// 也省略正常项的type字段
item = new nw.MenuItem({ label: '简单菜单项' });

// 为菜单项绑一个点击回调
item = new nw.MenuItem({
  label: "点我",
  click: function() {
    console.log("你刚刚点了我");
  },
  key: "s",
  modifiers: "ctrl+alt",
});

// 还可以创建子菜单!
var submenu = new nw.Menu();
submenu.append(new nw.MenuItem({ label: '菜单项 1' }));
submenu.append(new nw.MenuItem({ label: '菜单项 2' }));
submenu.append(new nw.MenuItem({ label: '菜单项 3' }));
item.submenu = submenu;

item.label = '新的菜单项';
item.click = function() { console.log('新菜单项的回调'); };
```

## new MenuItem(option)

* `option` Object - `MenuItem`初始设置对象
    - `label` String - (可选)  文本描述
    - `icon` String - (可选)  图标
    - `tooltip` String - (可选)  悬停文本提示
    - `type` String - (可选)  可选类型: `normal`, `checkbox`, `separator`
    - `click` Function - (可选)  键盘或鼠标点击的响应回调
    - `enabled` Boolean - (可选)  启用或禁用本项,默认 `true`即启用
    - `checked` Boolean - (可选)  是否勾选复选框,默认 `false`即不勾选
    - `submenu` Menu - (可选)  本项的子菜单项
    - `key` String - (可选)  本项的快捷键
    - `modifiers` String - (可选)  本项的快捷键提示

 `MenuItem` 源自 `EventEmitter`.您可以使用 `on`进行监听,菜单项属性详情请参考下文.

## item.type
> 用途:**设置或获取 `MenuItem`对象中设置的可选类型: `normal`, `checkbox`, `separator`**

请注意,类型仅在创建时进行设置,无法在已运行时更改.

## item.label
> 用途:**设置或获取 `MenuItem`对象中设置的 `label`文本描述**

## item.icon
> 用途:**设置或获取 `MenuItem`对象中设置的 `icon`图标**

  `icon`即图标文件路径,可以指向应用内置图标的相对路径,也可以是系统中的文件的绝对路径
  
请注意, `separator` 类型的菜单项图标是无效的.

## item.iconIsTemplate (Mac)
> 用途:**设置或获取 `MenuItem`对象中设置的  `icon`是否模板图标, 默认 `true`**

设置为 `true`时, `icon` 将被视为 `模板图像`,可配合系统状态(如灰暗/明亮模式)等自动确保所需样式.板图像应该仅由黑色和清晰的颜色组成，并且可以使用图像中的Alpha通道来调整黑色内容的不透明度。

## item.tooltip (Mac)
> 用途:**设置或获取 `MenuItem`对象中设置的 `tooltip`悬停文本提示**

 `tooltip`即鼠标悬停在菜单项时希望显示的内容提示,它是 `MenuItem`中的一个String属性.
 
## item.checked
> 用途:**设置或获取 `MenuItem`对象中设置的 `checked`是否勾选复选框**

左侧有个指示本项是否已勾选的标记

## item.enabled
> 用途:**设置或获取 `MenuItem`对象中设置的 `enabled`启用或禁用本项**

禁用后,菜单项文本变灰并且无法点击和响应.

## item.submenu
> 用途:**设置或获取 `MenuItem`对象中设置的 `submenu`子菜单项**

 `submenu`也是个 `Menu` 对像,子菜单项应该在创建整个菜单进行设置而非已运行时进行修改,因为某些平台这样的修改会很卡. 

## item.click
> 用途:**设置或获取 `MenuItem`对象中设置的 `click`键盘或鼠标点击的响应回调**

## item.key

> 用途:**设置或获取 `MenuItem`对象中设置的 `key`快捷键**

### 所有平台的有效键

* `a`-`z`
* `0`-`9`
* `[` , `]` , `'` , `,` , `.` , `/` , `` ` `` , `-` , `=` , `\` , `'` , `;` , `Tab`
* `Esc`
* `Down` , `Up` , `Left` , `Right`
* [W3C DOM Level 3 KeyboardEvent Key Values](http://www.w3.org/TR/DOM-Level-3-Events-key/): `KeyA` (等同`A`), `Escape` (等同`Esc`), `F1`, `ArrowDown` (等同 `Down`) 等等.

### Mac系统专用键
可使用 `String.fromCharCode(specialKey)`.调用专用键,列表如下:

* **28**: Left (<kbd>&larr;</kbd>)
* **29**: Right (<kbd>&rarr;</kbd>)
* **30**: Up (<kbd>&uarr;</kbd>)
* **31**: Down (<kbd>&darr;</kbd>)
* **27**: Escape (<kbd>&#9099;</kbd>)
* **11**: PageUp (<kbd>&#8670;</kbd>)
* **12**: PageDown (<kbd>&#8671;</kbd>)

有关专用键相关列表,请参阅:
* [NSMenuItem.keyEquivalent](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSMenuItem_Class/#//apple_ref/occ/instp/NSMenuItem/keyEquivalent)
* [NS事件: Function-Key Unicodes](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSEvent_Class/index.html#//apple_ref/doc/constant_group/Function_Key_Unicodes).

## item.modifiers
> 用途:**设置或获取 `MenuItem`对象中设置的 `modifiers`快捷键提示**

快捷键提示是当组合键时以 `+`相连的文本串,例如: `cmd` , `command`, `super`, `shift`, `ctrl`, `alt`, `"cmd+shift+alt"`.

 `cmd`在各平台的差异: Windows 和 Linux中的Windows key (<kbd>Windows</kbd>) ,Mac中是Apple key (<kbd>&#8984;</kbd>) . 
 
 `super` 和 `command`都是 `cmd`的别名.

## 事件: click
> 触发:**点击菜单项时**