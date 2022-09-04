[首页 | Read the Docs](https://readthedocs.org/)

参考：

[首页 | Read the Docs](https://readthedocs.org/)

https://www.jianshu.com/p/8aae1c1453ae/

# Sphinx教程

## 简介

Sphinx是一个文档生成工具，最初是为了生成python的帮助文档，现在已经支持很多编程语言了。

使用reStructuredText作为其标记语言，能够将rst文件进行美化排版并支持html、LaTeX、文本等格式的输出，非常合适作为项目的帮助文档生成工具。典型的ROS2的官方文档就是使用它生成的。



配置信息相关：https://www.osgeo.cn/sphinx/usage/configuration.html#confval-source_suffix

## 安装

安装sphinx

```shell
$pip install Sphinx                 
# 或者python3版本 
$pip3 install Sphinx
```

> 推荐使用国内镜像下载：
>
> ```
> $ pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple Sphinx
> ```

安装主题

```shell
$pip install sphinx_rtd_theme
```

## 基本命令

- 创建sphinx环境: sphinx-quickstart
- 生成html文档: make html
- 生成项目文档: sphinx-build source/ build/

```shell
$ cd docs/
$ 
```

## 基本使用

### 创建sphinx环境目录

一般我们会先建立一个docs文件夹，然后通过命令`sphinx-quickstart`创建环境目录。

```shell
$ cd docs/
$ sphinx-quickstart
```

其目录结构如下：

```shell
|-- docs
	|-- build\  #执行make html后，生成的html静态文件都存放在这里
    |-- source\
    	|-- _static\    #图片,js等存放地址
    	|-- _templates\ #模板文件存放
    	|-- conf.py     #配置文件
    	|-- index.rst   #索引文件,文章目录大纲
    |-- make.bat
    |-- Makefile
```

### 编译

```shell
$ cd docs/
$ make html
```

编译完成后，在build文件夹内便可以生成一个`index.html`文件，作为帮助文档的主页，可以直接打开。

## 使用markdown

1. 安装 Markdown 分析程序

   ```shell
   $ pip3 install --upgrade recommonmark
   # 使用国内镜像
   $ pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple  --upgrade recommonmark
   ```

2. 配置设置

   在config.py文件，进行如下配置：

   1）增加扩展：

   ```python
   extensions = [
       'sphinx.ext.autodoc',
       'recommonmark'          #增加扩展
   ]
   ```

   2）增加对扩展名解析

   可以在extensions后面增加：

   ```python
   source_suffix = {
       '.rst': 'restructuredtext',
       '.txt': 'restructuredtext',
       '.md': 'markdown',
   }
   ```

3. 添加md文件

   在docs/source下添加一个README.md文件，并且在index.rst中增加这个md文件索引：

   ```rst
   .. toctree::
      :maxdepth: 2
      :caption: 导航:
   
      README.md
   ```


> 注意，README.md一定在前面在空行！

## 示例

### 生成python帮助文档

我们有一个python程序文件helloworld.py，希望生成它的帮助文档。

#### 文档目录

首先创建目录结构如下：

```shell
|-- demo\
    |-- docs\             # sphinx的环境目录
    |-- helloworld.py     # 要生成帮助文档的python文件
```

#### 创建环境

```shell
$ cd docs/
$ sphinx-quickstart
```

该命令会配置sphinx的基本环境，根据提示输入配置选项即可。例如配置如下：

```shell
> Separate source and build directories (y/n) [n]: y   #
> Project name: demo                                   # 工程名字
> Author name(s): alex                                 # 作者
> Project release []: v1.0                             # 设置版本
> Project language [en]: zh-CN                         # 选择中文

```

然后会在docs目录下生成如下结构：

```shell
|-- docs
	|-- build\  #执行make html后，生成的html静态文件都存放在这里
    |-- source\
    	|-- _static\    #图片,js等存放地址
    	|-- _templates\ #模板文件存放
    	|-- conf.py     #配置文件
    	|-- index.rst   #索引文件,文章目录大纲
    |-- make.bat
    |-- Makefile
```

#### 文件配置

**conf.py**

```python
"""
1.设置根目录，以此目录为根去索引各文件，这里我们的helloworld.py文件在conf.py的上一级的再上一级目录。
"""
import os
import sys
sys.path.insert(0, os.path.abspath('../..'))
"""
3. 设置扩展
"""
extensions = [
    'sphinx.ext.autodoc'
]
"""
2. 设置主题，也就是网页主题风格，这个风格主题更好看一些。
"""
html_theme = 'sphinx_rtd_theme'
```

**index.rst**

这个会作为文档“入口”。

```rst
HelloWorld
==========
.. automodule:: helloworld
   :members:
```



## 函数注释格式说明

```python
def calculate_haversin_distance(gps1, gps2):
    """
    计算大地坐标系（WGS84）下两个经纬度坐标点直接的距离，计算的是地表弧长。
    Calculate distance(in meter) between two global position(WGS84), use Haversine way.

    Inputs:
        - gps1: a tuple, first position
        - gps2: a tuple, secend position
    Outputs:
        distance: a value, distance(in meter) between the position
    """
    pass
```



我们的工程目录结构如下

```python
|-- onboardSDK/
    |-- onboardDevSDK/
        |-- utils/
            |-- rotation_transform.py
        |-- drone_api.py
        |-- mav_api.py
```



## 问题和解决

### 无法导入模块

问题描述：

WARNING: autodoc: failed to import module 'drone_api'; the following exception was raised:
attempted relative import with no known parent package

解决方法：

注意查看，在模块中使用import引用其他模块时是否使用了相对导入方式：

```python
from .mav_api import *
```

请修改为绝对导入：

```python
from mav_api import *
```

