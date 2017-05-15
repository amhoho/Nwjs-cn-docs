# 快捷键

`Shortcut`表示全局快捷键或系统级热键. 注册成功后,无需聚焦也可以工作.

`Shortcut`继承自[`EventEmitter`](https://nodejs.org/api/events.html#events_class_events_eventemitter). 每当用户按下所注册的快捷键,应用都将收到快捷对象中的 `active`事件.

## 示例

```js
var option = {
  key : "Ctrl+Shift+A",
  active : function() {
    console.log("全局快捷键: " + this.key + " 被激活."); 
  },
  failed : function(msg) {
    // :(, 无法注册 |key| 或未注册 |key|.
    console.log(msg);
  }
};

// 使用 |option| 注册快捷键
var shortcut = new nw.Shortcut(option);

// 注册全快捷键 即使无聚焦也可工作
nw.App.registerGlobalHotKey(shortcut);


//注册后,用户按下Ctrl + Shift + A时,应用将收到 `active`事件.

// 您还可以监听快捷键的成功或失败事件
shortcut.on('active', function() {
  console.log("Global desktop keyboard shortcut: " + this.key + " active."); 
});

shortcut.on('failed', function(msg) {
  console.log(msg);
});

// 注销全局快捷键
nw.App.unregisterGlobalHotKey(shortcut);
```

## new Shortcut(option)
> 用途:**创建新的 `Shortcut`快捷键**

* `option` Object
    - `key`  String - 快捷键组合如 `"ctrl+shift+a"`. 细节请查阅 [shortcut.key](#shortcutkey)属性
    - `active` Function(可选) - 触发热键时的回调. 细节请查阅 [shortcut.active](#shortcutactive) 属性
    - `failed` Function (可选) -  注册热键失败时的回调. 细节请查阅 [shortcut.failed](#shortcutfailed) 属性

## shortcut.key
> 属性:**`Shortcut`的 `key`,多个则采用 `+` 连接的快捷键组合,如 `"Ctrl+Alt+A"`.**

该键值以0个或多个功能键和普通键组成,功能键如 `alt`可忽略大小写.

可用的功能键:
* `Ctrl`
* `Alt`
* `Shift`
* `Command`: `Command`  Mac上即<kbd>&#8984;</kbd>, Windows和Linux上即Windows键.

可用的普通键:
* `0` 到 `9`
* `A` 到 `Z`
* `F1` 到 `F24`
* `Comma`
* `Period`
* `Tab`
* `Home` / `End` / `PageUp` / `PageDown` / `Insert` / `Delete`
* `Up` / `Down` / `Left` / `Right`
* `MediaNextTrack` / `MediaPlayPause` / `MediaPrevTrack` / `MediaStop`
* `Comma` 或 `,`
* `Period` 或 `.`
* `Tab` 或 `\t`
* `Backquote` 或 `` ` ``
* `Enter` 或 `\n`
* `Minus` 或 `-`
* `Equal` 或 `=`
* `Backslash` 或 `\`
* `Semicolon` 或 `;`
* `Quote` 或 `'`
* `BracketLeft` 或 `[`
* `BracketRight` 或 `]`
* `Escape`
* [DOM Level 3 W3C KeyboardEvent Code Values](http://www.w3.org/TR/DOM-Level-3-Events-code/)

虽然 `App.registerGlobalHotKey()`可将普通键如 `A`注册成一个快捷键,但很少人会有这种需求,但API本身不会限制此类做法,因为你可能会希望用它监听某个按键

## shortcut.active
> 属性:**用户按下快捷键时获取或设置 `Shortcut`的 `active`回调**

## shortcut.failed
> 属性:**快捷键无法注册或注册失败时获取 `Shortcut`的 `failed`回调**

## 事件:active
> 触发:**用户按下快捷键时**

参考上文[`shortcut.active`](#shortcutactive)

## 事件:failed
> 触发:**快捷键无法注册或注册失败时**

参考上文[`shortcut.failed`](#shortcutfailed)
