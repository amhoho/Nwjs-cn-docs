# 屏幕

> `Screen`是个单例对象,使用前需要调用一次 `nw.Screen.Init()`进行启动,`Screen.on(...)`可对屏幕事件进行监听

## 示例

```javascript
//启动时必须进行一次调用,之后便可调用任何nw.Screen函数了
nw.Screen.Init();

var screenCB = {
  onDisplayBoundsChanged: function(screen) {
    console.log('displayBoundsChanged', screen);
  },
  onDisplayAdded: function(screen) {
    console.log('displayAdded', screen);
  },
  onDisplayRemoved: function(screen) {
    console.log('displayRemoved', screen)
  }
};

//监听屏幕边界与增删
nw.Screen.on('displayBoundsChanged', screenCB.onDisplayBoundsChanged);
nw.Screen.on('displayAdded', screenCB.onDisplayAdded);
nw.Screen.on('displayRemoved', screenCB.onDisplayRemoved);
```

## Screen.Init()
> 用途:**启动时初始屏幕单例对象,只需要调用一次**

## Screen.screens
> 用途:**获取连接到本机的屏幕数量**

屏幕具有以下结构:

```javascript
screen {
// 屏幕的唯一ID
  id: int,

//物理屏幕分辨率，不一定从0开始也可以是负数，这取决于屏幕排列
  bounds: {
    x: int,
    y: int,
    width: int,
    height: int
  },
 
// 屏幕范围内的可用区域
  work_area: {
    x: int,
    y: int,
    width: int,
    height: int
  },

  scaleFactor: float,
  isBuiltIn: bool,
  rotation: int,
  touchSupport: int
}
```

## Screen.chooseDesktopMedia (sources, callback) _Windows_ _Linux_
> 用途:**选择屏幕进行共享**

* `sources` String[] - 源类型数组,可选: `"screen"` 和 `"window"`.
* `callback` Function - 已选streamId的回调函数. streamId为 `false`即意味着执行失败或现有session存活.

示范:
```javascript
nw.Screen.Init(); // 启动一次
nw.Screen.chooseDesktopMedia(["window","screen"], 
  function(streamId) {
    var vid_constraint = {
      mandatory: {
        chromeMediaSource: 'desktop', 
        chromeMediaSourceId: streamId, 
        maxWidth: 1920, 
        maxHeight: 1080
      }, 
      optional: []
    };
    navigator.webkitGetUserMedia({audio: false, video: vid_constraint}, success_func, fallback_func);
  }
);
```

## 事件: displayBoundsChanged(screen)
> 触发:**更改屏幕分辨率时,可带有`screen`([Screen.screens](#screenscreens))的进行回调**

## 事件: displayAdded (screen)
> 触发:**添加新屏幕时,可带有`screen`([Screen.screens](#screenscreens))的进行回调**

## 事件: displayRemoved (screen)
> 触发:**移除新屏幕时,可带有`screen`([Screen.screens](#screenscreens))的进行回调**

## Screen.DesktopCaptureMonitor
> 用途:**使用此API监视桌面上的屏幕和窗口的更改，并实现自己的UI**

该API与 `Screen.chooseDesktopMedia`类似的功能,但它没有GUI.

 `Screen.DesktopCaptureMonitor`是个 `EventEmitter`实例. 可通过 `Screen.DesktopCaptureMonitor.on()`进行监听.

### 示例

```javascript
var dcm = nw.Screen.DesktopCaptureMonitor;
nw.Screen.Init();
dcm.on("added", function (id, name, order, type) {
   //选择第一个stream并关闭
   var constraints = {
      audio: {
         mandatory: {
             chromeMediaSource: "system",
             chromeMediaSourceId: dcm.registerStream(id)
          }
      },
      video: {
         mandatory: {
             chromeMediaSource: 'desktop',
             chromeMediaSourceId: dcm.registerStream(id)
         }
      }
  };

  //用约束调用getUserMedia之类的后续操作

  dcm.stop();
});

dcm.on("removed", function (id) { });
dcm.on("orderchanged", function (id, new_order, old_order) { });
dcm.on("namechanged", function (id, name) { });
dcm.on("thumbnailchanged", function (id, thumbnail) { });
dcm.start(true, true);
```

### Screen.DesktopCaptureMonitor.started
> 用途:**判断 `DesktopCaptureMonitor`桌面捕获监视器是否已启动( `Boolean`)**

### Screen.DesktopCaptureMonitor.start(should_include_screens, should_include_windows)
> 用途:**使`DesktopCaptureMonitor`桌面捕获监视器开始监视系统并触发事件**

* `should_include_screens` Boolean - 是否包括屏幕
* `should_include_windows` Boolean - 是否包括窗口

如果 `DesktopCaptureMonitor`桌面捕获监视器正在运行，屏幕可能会闪烁

### Screen.DesktopCaptureMonitor.stop()
> 用途:**使`DesktopCaptureMonitor`桌面捕获监视器停止始监视系统**

当选择了stream后 `DesktopCaptureMonitor`桌面捕获监视器应该被停止.

### Screen.DesktopCaptureMonitor.registerStream(id)
> 用途:**注册并返回在 `getUserMedia`约束中传递给 `chromeMediaSourceId`的有效stream ID**

 相关细节,请参阅 [Synopsis](#synopsis_1)
 
### 事件: added (id, name, order, type, primary)
> 触发:**添加源的时候**

* `id` String - 媒体ID. 使用 `registerStream(id)`可获取与 `getUserMedia()`一同使用的有效stream ID. 详细参阅上文[registerStream](#screendesktopcapturemonitorregisterstreamid)
* `name` String -窗口或屏幕的标题
* `order` Integer - 窗口的z顺序
* `type` String - 流的类型,可选值: `screen`, `window`, `other` 或 `unknown`
* `primary` Boolean - _Windows_ 如果源是主监视器时,该项为 `true`

### 事件: removed (order)
> 触发:**删除源的时候**

* `order` Integer - 媒体源的顺序

### 事件: orderchanged (id, new_order, old_order)
> 触发:**变更源的Z顺序的时候**

* `id` String - 更改了z顺序的屏幕或窗口的媒体ID
* `new_order` Integer - 新的z顺序
* `old_order` Integer - 旧的z顺序

### 事件: namechanged (id, name)
> 触发:**变更源的名称(如窗口更改了标题时)的时候**

* `id` String - 更改了名称的屏幕或窗口的媒体ID
* `name` String - 屏幕或窗口的新名称

### 事件: thumbnailchanged (id, thumbnail)
> 触发:**变更源的缩略图的时候**

* `id` String - 更新了缩略图的屏幕或窗口的媒体ID
* `thumbnail` String - 缩略图png的base64编码