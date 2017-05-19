# 在NW.js中使用NaCl

NaCl需要在SDK和NaCl构建方式的NW中使用 , 参考[构建方式](Build-Flavors.md) .      

NW.js同Chromium一样支持NaCl(原生客户端)以及PNaCl(便携原生客户端) . 应用可以嵌入NaCl和PNaCl .

	以下文档说明由[Chrome NaCl 文档](https://developer.chrome.com/native-client/devguide/tutorial/tutorial-part1)提供 . 

## 概述

该文档说明了如何使用PNaCl构建和运行Web应用 . 该客户端应用使用HTML , JavaScript和C++编写的原生模块进行开发 . 

页面能够使用PNaCl提供的工具运行原生客户端模块 . 

阅读该文档说明之前 , 建议先了解[原生客户端技术](https://developer.chrome.com/native-client/overview.html)相关信息 .        

### 文档应用需求
文档中的应用展示如何在页面加载一个原生客户端模块 , 如何在JavaScript和原生客户端模块之间发送消息 . 

该应用使用JavaScript发送 `'hello'`信息给原生客户端模块 . 当原生客户端模块接收到信息之后 , 检查接收到的信息是否为 `'hello'`字符串 . 如果是 , 原生客户端模块返回 `'hello from NaCl'`信息 . JavaScript接收到返回的信息之后进行打印 . 

### JavaScript和原生客户端模块通信

原生客户端模块支持双向通信 , 既能够在JavaScript和原生客户端模块之间进行通信 , 两端能够发起以及接收信息 . 所有通信过程都是异步完成:呼叫者(JavaScript或原生客户端模块)发送信息 , 但是呼叫者不会等待 , 甚至不期望响应 . 该过程类似于网页请求中的客户端和服务器之间的通信 , 客户端发送消息给服务器之后立刻返回 . 原生客户端通信系统是Pepper API的一部分 , 参考[通信系统开发者指南](https://developer.chrome.com/native-client/devguide/coding/message-system.html). 该[web worker](http://en.wikipedia.org/wiki/Web_worker)方式也是JavaScript中文档主要交互方式.

## 步骤 1: 下载以及安装原生客户端SDK

参考[说明](https://developer.chrome.com/native-client/sdk/download.html)下载及安装原生客户端SDK . 

## 步骤 2: 启动本地服务器

为了模拟生产环境，SDK提供了一个简单的Web服务器 , 可以通过 `localhost`访问 .并通过Makefile调用 `serve`运行该服务:

```bash
$ cd pepper_$(VERSION)/getting_started
$ make serve
```

	SDK中可能包含多个Chrome/Pepper版本 , 参考[版本](https://developer.chrome.com/native-client/version.html) . 	可以使用 `pepper_$(VERSION)`调用指定版本 , 如 `pepper_37` . 
	如果不知道使用哪个版本 , 可以通过 `naclsdk list`命令查看稳定版本 . 参考[原生客户端SDK下载](https://developer.chrome.com/native-client/sdk/download.html) .          
	
如果没有指定端口 , 服务默认端口为 `5103` , 可以使用 `http://localhost:5103`进行访问 . 

提供的服务目的是为了开发 , SDK并不是必须的 . 

## 步骤 3: 设置Chrome浏览器

PNaCl默认开启Chrome浏览器 . 建议使用与Chrome浏览器相同版本 , 或者版本高于SDK构建原生客户端模块 . 低版本PNaCl模块能够在高版本的Chrome浏览器中工作 , 反之将不能正常工作 . 

	可以在浏览器中输入 `about:chrome`查看Chrome浏览器版本信息 . 

根据开发经验 , 建议关闭Chrome浏览器缓存 . 缓存资源优先级高 ; 关闭缓存能够确保最新版本的本地模块能够被加载 . 

* [`菜单`](https://developer.chrome.com/native-client/images/menu-icon.png) &gt; `工具` &gt; `开发者工具` , 打开Chorme浏览器的开发者工具 . 
* 点击Chrome窗口右下角[齿轮图标](https://developer.chrome.com/native-client/images/gear-icon.png)按钮 .      
* 通用设置中 , `禁用缓存(当DevTools打开时)`选中 . 
* 开发原生客户端应用过程中 , 一直开启开发者工具面板 . 

## 步骤 4: 样例代码
样例代码在SDK中可以使用 , 代码在路径 `pepper_$(VERSION)/getting_started/part1` . 包含以下文件:

* `index.html`: 包括页面样式布局 , JavaScript与原生客户端模块之间通信 . 原生客户端包含在 `<embed>`标签中 , 指向一个配置文件 . 
* `hello_tutorial.nmf`: 配置文件 , 指向HTML文件以及原生客户端模块 , 同时可以额外为PNaCl配置命令 , 作为Chrome浏览器的一部分 . 
* `hello_tutorial.cc`: 原生客户端模块的C++代码 . 
* `MakeFile`: 编译文件 , 将 `hello_tutorial.cc`中的C++代码编译为**pexe**(便携可执行文件)

样例代码中有大量的说明解析代码的结构以及内容 . 更多关于原生客户端应用结构的描述 , 参考[应用结构](https://developer.chrome.com/native-client/devguide/coding/application-structure.html)

样例代码有意简短编写 . C++代码除了正确初始化 , 并没有完成其他工作 . JavaScript代码等待原生客户端模块的加载以及将状态信息显示在页面中 . 

## 步骤 5: 编译原生客户端模块以及运行样例应用
使用`make`命令编译原生客户端模块:

```bash
$ cd pepper_$(VERSION)/getting_started/part1
$ make
```

样例包含在SDK目录结构中 , Makefile编译文件知道如何使用PNaCl工具自动完成编译工作 . 
如果需要在SDK目录结构之外完成编译工作 , 需要设置 `$NACL_SDK_ROOT`环境变量支持 . 参考[构建原生客户端模块](https://developer.chrome.com/native-client/devguide/devcycle/building.html) .                   

假设使用[步骤 2](#step-2-start-a-local-server)开启了本地服务 , 可以在浏览器中输入http://localhost:5103/part1加载样例应用 . Chrome浏览器应该成功加载本地模块 , 并且页面中状态信息显示从 `LOADING...`变为 `SUCCESS` . 如果运行出错 , 使用[排查步骤](#troubleshooting)进行查看 . 


## 步骤 6: JavaScript给原生客户端发送消息
该步骤中 , 修改网页文件( `index.html`)完成页面加载模块之后能够发送消息给原生客户端 . 

找到JavaScript函数 `moduleDidLoad()` , 添加发送 `hello` 信息代码 , 如下:

```javascript
function moduleDidLoad() {
  HelloTutorialModule = document.getElementById('hello_tutorial');
  updateStatus('SUCCESS');
  // 发送消息给原生客户端模块
  HelloTutorialModule.postMessage('hello');
}
```

## 步骤 7: 原生客户端实现信息处理
该步骤中 , 需要修改原生客户端模块 `hello_tutorial.cc` , 接收JavaScript发送的信息 , 之后响应相应的信息 . 特别说明:

* 模块实例中实现 `HandleMessage()`方法
* 使用 `PostMessage()`方法从模块发送信息给JavaScript代码 . 

首先 , 定义原生客户端模块中需要使用的变量 , `hello`字符串为期望从JavaScript接收到的信息 , `hello from NaCl`字符串为原生客户端返回给JavaScript的字符串 . `hello_tutorial.cc`文件中 , #include代码快之后加入一下代码:

```c++
namespace {
// 浏览器发送字符串
const char* const kHelloString = "hello";
// 接收信息之后返回给浏览器信息
const char* const kReplyString = "hello from NaCl";
} // 命名空间
```

现在 , 实现 `HandleMessage()`方法检查 `kHelloString`变量内容以及将 `kReplyString`作为结果进行返回 , 如下:

```c++
virtual void HandleMessage(const pp::Var& var_message) {
  if (!var_message.is_string())
    return;
  std::string message = var_message.AsString();
  pp::Var var_reply;
  if (message == kHelloString) {
    var_reply = pp::Var(kReplyString);
    PostMessage(var_reply);
  }
}
```

参考Pepper API文档 , 关于成员方法[pp::Instance.HandleMessage](https://developer.chrome.com/native-client/pepper_stable/cpp/classpp_1_1_instance.html#a5dce8c8b36b1df7cfcc12e42397a35e8)和[pp::Instance.PostMessage](https://developer.chrome.com/native-client/pepper_stable/cpp/classpp_1_1_instance.html#a67e888a4e4e23effe7a09625e73ecae9)相关信息 . 

## 步骤 8: 编译原生客户端模块及运行应用

1. 再次使用 `make`命令编译原生客户端模块 . 
2. 运行 `make serve`开启SDK服务 . 
3. Chrome浏览器中重新加载http://localhost:5103/part1地址运行应用 . 

Chrome浏览器加载原生客户端后 , 应该能够查看到信息由模块中发送 .

## 问题排查

如果应用不能运行 , 检查[步骤 3](#step-3-set-up-the-chrome-browser) , 是否使用正确的环境 , 包括浏览器以及本地服务 . 确保当前运行的Chrome浏览器版本大于或者等于当前SDK版本 . 

另外可以使用Chrome提供的JavaScript调试控制台 , 可以在Chrome工具菜单中进行配置 . 检查不能正确运行的原因 . 比如 , 信息如果是`NaCl module crashed`, 可能由于原生客户端模块存在问题 . [调试](https://developer.chrome.com/native-client/devguide/devcycle/debugging.html)功能是使用该功能所必须的 . 

更多关于问题排查的信息文档:

* [问题排查FAQ](https://developer.chrome.com/native-client/faq.html#faq-troubleshooting).
* [Progress Events](https://developer.chrome.com/native-client/devguide/coding/progress-events.html)文档有关于问题事件解决相关信息 . 

## 下一步

* 参考开发者指南中[应用结构](https://developer.chrome.com/native-client/devguide/coding/application-structure.html)部分 , 获取关于如何构建原生客户端模块的相关说明 . 
* 参考[C++参考](https://developer.chrome.com/native-client/pepper_stable/cpp)获取关于如何使用Pepper APIs的相关说明 . 
* 查看SDK样例代码学习使用Pepper APIs编写其他功能的方法 . 
* 参考[Building](https://developer.chrome.com/native-client/devguide/devcycle/building.html) , [Running](https://developer.chrome.com/native-client/devguide/devcycle/running.html)以及[Debugging pages](https://developer.chrome.com/native-client/devguide/devcycle/debugging.html)如何完成构建 , 运行以及调试原生客户端应用 . 
* 参考[naclports](http://code.google.com/p/naclports/)项目 , 提供使用其他可用原生客户端库 . 如果应用使用开源库 , 推荐使用naclport完成 . 

_以上内容适用[CC-By 3.0 license](http://creativecommons.org/licenses/by/3.0/)_