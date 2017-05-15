# 提交应用至Mac App Store

您可以将基于NW.js的应用程序提交至MAS(Mac App Store),但前提必须在发行前进行应用签名.下文将为您展示这一过程.

## 提交前提

* 通过[iTunesConnect](https://itunesconnect.apple.com)创建一个macOS应用程序
* 从[Apple Developer](https://developer.apple.com).获取申请与开发者证书
    - 如果您通过*Mac App Store**分发:
        + 第三方Mac开发者应用程序：Foo (XXXXXXXXXX)
        + 第三方Mac开发人员开发者：Foo (XXXXXXXXXX)
    - 如果是在**MAS店外**分发:
        + 开发者ID申请：Foo (XXXXXXXXXX)
        + 开发者安装ID申请：Foo (XXXXXXXXXX)

## 建立应用

从[nwjs.io](http://dl.nwjs.io/v0.19.5-mas-beta/)下载NW.js MAS构建并进行[打包与发布](../Package-and-Distribute.md).         

## 签名应用

 `build_mas.py` 将用于签名应用. 该脚本通过给定 `.pkg`参数签名后,为MAS生成一个可上传的 `--pkg`文档.

 **基本使用**
 
```bash
python build_mas.py -C build.cfg -I myapp-dev.app -O MyApp.app
```

## 配置文件格式

配置文件(`build.cfg`) 是一个人类可阅读的文本文档. 它包含了用于签名和打包应用程序的重要设置.

 `ApplicationIdentity` 和 `InstallerIdentity` 是用于签名和打包应用的名称. 有关所需证书,请参阅 [提交前提](#prerequisits).      
 
 `NWTeamID` 用于建立基于NW.js应用程序的IPC通道。它可以从Apple Developer -> Membership -> Team ID获取.

`ParentEntitlements` 和 `ChildEntitlements` 应该是有效的[授权文件](https://developer.apple.com/library/content/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html). 默认以最小权限进行签名,如下所示:

**entitlements-parent.plist**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>com.apple.security.app-sandbox</key>
  <true/>
  <key>com.apple.security.application-groups</key>
  <string>NWTeamID.your.app.bundle.id</string>
</dict>
</plist>
```

**entitlements-child.plist**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>com.apple.security.app-sandbox</key>
  <true/>
  <key>com.apple.security.inherit</key>
  <true/>
</dict>
</plist>
```

相关细节含义,请阅读 `build.cfg`的示例，以获取详细的含义