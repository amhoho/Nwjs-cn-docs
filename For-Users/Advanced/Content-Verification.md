# 内容验证
>"内容验证"功能或"应用签名"可阻止正式应用中加载未签名文档

 `'verified_contents.json'`文件为应用的签名文件,它提供了密匙对,该文件由 `'sign.py'`工具与私钥文件 `private_key.pem`所创建,公钥已经内置于NW.js应用件中.

要运行签名的应用程序，请在应用程序目录中调用`nw --verify-content=enforce_strict .`命令.之后显示签名页面 , 完成签名动作 .

在此之后对 `index.html`进行的任意修改,NW将报告文件已损坏并立即退出。

!!! 注意
    内容验证或者应用签名并不能阻止别有用心的人黑进你的应用 , 或者使用其他NW代码加载你的应用 . 可以考虑使用C++编写 , 并使用Node.js模块和NaCl加载 , 以及[nwjc编译JS代码](Protect-JavaScript-Source-Code.md)        

## 应用签名

使用密钥对签名应用 , 步骤如下:

1. 切换到应用目录中 . 
2. 确认`verified_contents.json` 或 `computed_hashes.json`文件不在当前目录 , 如果存在删除 . 
3. 运行 `payload.exe`生成 `sign.py`所需的配置文件 `payload.json`文件
4. 运行 `python sign.py > /tmp/verified_contents.json` , 需要注意tmp目录不能是应用所在目录 . 
5. 将生成的 `verified_contents.json`文件拷贝到应用目录 , 完成签名动作 . 

## 通过密钥对重新构建应用

要使用您自己的密钥对,您需要重建NW. 并命令行参数  `--verify-content=`默认设置为 `enforce_strict`

1. `openssl genrsa -out private_key.pem 2048`生成密钥对 , 输出公钥和私钥两个文件
2. 运行 `python convertkey.py` , 它会将公钥转换为C源代码.
3. 将生成的代码复制到 `content/nw/src/nw_content_verifier_delegate.cc` , 替换原文件的默认key值 . 
4. 更改文件中第73行为 `Mode experiment_value =  ContentVerifierDelegate::ENFORCE_STRICT;`
5. 重新构建NW

工具中 , 样例应用和样例私钥在 `tools/sign`目录中 . 该样例私钥匹配官方NW中的内置公钥