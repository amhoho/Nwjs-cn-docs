# 透明窗体
> 创建可透明的无框窗口

### Windows

至少是Vista并开启DWM玻璃特效的系统中才可使用透明窗体,其它如经典注意或XP或远程桌面中均不可使用.

### Linux

NW.js运行需要特定参数 , 同时窗口关系需要支持 **compositing**:

```params
--enable-transparent-visuals --disable-gpu
```

## 创建透明窗体

HTML代码中指定背景颜色参数 `alpha`:
```html
<body style="background-color:rgba(0,0,0,0);">
```

配置文件中设置[`transparent`属性](../../References/Manifest-Format.md#transparent)为 `true`:
```json
  "window": {
    "frame": false,
    "transparent": true
  }
```

## 点击链接 (Windows and Mac) 

Windows和Mac系统中 , 可以开启点击链接透明度 . 该特性控制在窗口点击对象后`alpha`值为0 . 

开启点击链接透明度 , 需要使用命令行参数:
```params
--disable-gpu --force-cpu-draw
```

注意, **无框架**和 **不可调整大小框架**均支持点击链接 , 但还需要运行系统的相关配置 . 