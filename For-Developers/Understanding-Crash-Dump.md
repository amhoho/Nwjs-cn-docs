# 崩溃机制

NW.js崩溃时 , 将在桌面创建名为 `.dmp`格式的`minidump`报告,解码 `minidump`文件提取堆栈跟踪可查看问题详情。

解码 `minidump`文件提取堆栈跟踪,您需要三个条件: 

* 崩溃时产生的`minidump`文件(`.dmp`)
* NW.js的Symbol文件(`.sym`)
* `minidump_stackwalk`工具

## 获取minidump文件

NW.js崩溃时在不同系统中生成minidump的路径:

* Linux: `~/.config/<name-in-manifest>/Crash\ Reports/`
* Windows: `%LOCALAPPDATA%\<name-in-manifest>\User Data\CrashPad` 
* Mac: `~/Library/Application\ Support/<name-in-manifest>/CrashPad/`

上文的 `<name-in-manifest>`是 [配置文件](../References/Manifest-Format.md#name)文件中的 `name`字段.

!!! "Linux Minidump文件中的Strip标头"需知:
    Linux系统中生成的minidump文件会附带头文本信息,解码之前必须将其剥离。真正minidump文件内容由 `MDMP`开头，之前的内容是不可读的 , 可以直接删除。
    
## 整理Symbol文件(`.sym`)

您可在NW.js下载包中找到发布的NW.js的Symbol文件. 该(.sym)文件可从下载包中提取; 
注意,该(.sym)文件必须正确命名以便使用 `minidump_stackwalk` . `minidump_stackwalk` 使用了 [simple symbol supplier](https://code.google.com/p/chromium/codesearch#chromium/src/breakpad/src/processor/simple_symbol_supplier.cc&l=142) 来查找 symbol 文件. 以下是查找symbol 文件的方式

该工具将尝试按照以下模式搜索`.sym`文件:
`{SYMBOLS_ROOT}/{DEBUG_FILE_NAME}/{DEBUG_IDENTIFIER}/{DEBUG_FILE_NAME_WITHOUT_PDB}.sym`

* `{SYMBOLS_ROOT}` 所有symbol 文件的根目录. NW.js所有版本或者使用系统都可以将 `.sym`文件放入该目录中 . 
* `{DEBUG_FILE_NAME}`, `{DEBUG_IDENTIFIER}` 和 `{DEBUG_FILE_NAME_WITHOUT_PDB}` 可从 `.sym` 文件的首行获取,如: `MODULE Linux x86_64 265BDB6BE043D5C70D3A1E279A8F0B1A0 nw`.
    - `265BDB6BE043D5C70D3A1E279A8F0B1A0` 对应 `{DEBUG_IDENTIFIER}`
    - `nw` 对应 `{DEBUG_FILE_NAME}`.
    - `{DEBUG_FILE_NAME_WITHOUT_PDB}` 可覆盖删除 `{DEBUG_FILE_NAME}` 中的 `.pdb`扩展, 该方式只有在Windows系统中需要 . 

## 使用 `minidump_stackwalk` 解码 Minidump 

 `minidump_stackwalk`能够使用NW.js构建 . Linux和Mac系统的报告器代码中直接构建 . Windows系统中使用Cygwin预制构建 .

要从`minidump` 文件获取堆栈跟踪，请运行以下命令:
 
```bash
minidump_stackwalk minidump_file.dmp /path/to/symbols_root 2>&1
```

如果条目文件不能正确整理 , 仍然可以使用该工具获取 . 但不能查看 , 最后部分会输出警告 - `Loaded modules`，如下所示：

```none
0x00240000 - 0x02b29fff nw.exe ??? (main) (WARNING: No symbols, nw.exe.pdb, 669008F7B6EE44058CBD5F21BEB5B5CFe)
```

## 触发崩溃以测试

测试崩溃机制特性 , NW.js提供APIs触发崩溃: `App.crashBrowser()` 和 `App.crashRenderer()`.  分别是崩溃浏览器进程和渲染进程 . 

## 参考文献

* http://www.chromium.org/developers/decoding-crash-dumps  
* http://code.google.com/p/google-breakpad/wiki/GettingStartedWithBreakpad