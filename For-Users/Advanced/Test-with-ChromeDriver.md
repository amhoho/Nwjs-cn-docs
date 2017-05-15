# ChromeDriver测试

> [WebDriver](https://sites.google.com/a/chromium.org/chromedriver/)是一种开源工具, 可跨浏览器并自动测试webapps . 它提供引导页面 , 用户输入 , 脚本执行等功能,实现Chromium协议的独立服务,可实现针对Chromium的WebDriver线程协议.
ChromeDriver适用于Android版Chrome和桌面版(Mac，Linux，Windows和ChromeOS)。

NW.js提供了一个自定义ChromeDriver来自动测试基于NW.js开发的应用 , 可通过类似于[selenium](http://docs.seleniumhq.org/)工具进行使用 . 

## 开始

以下过程是使用[selenium-python](http://selenium-python.readthedocs.org/)进行测试 . 也可以使用Selenium支持的其他语言调用 `chromedriver` . 

### 安装

* 从官网下载SDK构建方式的NW.js , ChromeDriver已内置其中 . 
* 解压下载的文件 , `chromedriver`在解压的NW.js目录中 , Linux系统中名为 `nw` , Windows系统中名为 `nw.exe` , Mac系统中名为 `node-webkit.app`
* 安装 `selenium-python`到python环境中 : 
```bash
pip install selenium
```

### 运行

假设你的应用远程查询内容 . 页面代码大致如下:
```html
<form action="http://mysearch.com/search" method="GET">
    <input type="text" name="q"><input type="submit" value="Submit">
</form>
```

编写一个python脚本自动填充搜索框并提交表单:
```python
import time
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument("nwapp=/path/to/your/app")

driver = webdriver.Chrome(executable_path='/path/to/nwjs/chromedriver', chrome_options=chrome_options)

time.sleep(5) # 等待5秒 , 查看页面
search_box = driver.find_element_by_name('q')
search_box.send_keys('ChromeDriver')
search_box.submit()
time.sleep(5) # 等待5秒 , 查看搜索结果
driver.quit()
```

关于 `selenium-python`.请参考[selenium-python文档](http://selenium-python.readthedocs.org).  

## 修改chromedriver工具

* 被修改的chromedriver工具默认在NW可执行文件同级目录下 . 

* 如果需要传递非switch参数给命令行 , 需要使用选项 `nwargs`:
```python
import time
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument("nwapp=/path/to/your/app")
chrome_options.add_experimental_option("nwargs", ["arg1", "arg2"])

driver = webdriver.Chrome(executable_path='/path/to/nwjs/chromedriver', chrome_options=chrome_options)
```