# PyQt6 简介
*最后更新于 2021.04.22*

本教程是 PyQt6 的入门教程。本教程的目的是让您开始使用 PyQt6 库。
## 关于 PyQt6

PyQt6 Digia 公司的 Qt 程序的 Python 中间件。Qt库是最强大的GUI库之一。PyQt6的官网：[www.riverbankcomputing.co.uk/news](https://www.riverbankcomputing.co.uk/news)。PyQt6是由Riverbank Computing公司开发的。~~Digia和RiverBank的关系不太清楚，大家多提意见~~

PyQt6 是基于 Python 的一系列模块。它是一个多平台的工具包，可以在包括Unix、Windows和Mac OS在内的大部分主要操作系统上运行。PyQt6 有两个许可证，开发人员可以在 GPL 和商业许可之间进行选择。

## 安装 PyQt6
```sh
$ pip install PyQt6
```
我们可以使用 `pip` 工具安装 PyQt6。

## PyQt6 模块
PyQt6 类是由一系列模块组成的，包括如下的模块：

- QtCore
- QtGui
- QtWidgets
- QtDBus
- QtNetwork
- QtHelp
- QtXml
- QtSvg
- QtSql
- QtTest

`QtCore` 模块是非 GUI 的核心库。这个模块用来处理时间、文件、目录、各种类型的数据、流（stream）、URLs，mime 类型、线程和进程。 `QtGui` 有窗口系统集成、事件处理、2D图形，基本图像、字体、文本的类。 `QtWidgets` 有创建经典风格的用户界面的类。

`QtDBus` 是使用 D-Bus 处理 IPC 通讯的类。`QtNetwork` 是网络变成类，这些类使网络编程变得更容易，可移植性也更好，方便了 TCP/IP 和 UDP 服务端和客户端编程。 `QtHelp` 包含了创建、查看和搜索文档的类。

`QtXml` 包含了处理 XML 文件的类，实现了 SAX 和 DOM API。`QtSvg` 提供了显示 SVG 的类，可缩放矢量图形(SVG)是一种描述二维图像和图像应用的 XML 语言。`QtSql` 模块提供了数据库的类，`QtTest` 提供了可以对 PyQt6 应用进行单元测试的工具。

## Python

Python 是一种通用的、动态的、面向对象的编程语言。Python语言的设计目的强调程序员的生产力和代码的可读性。它于1991年首次发行。Python 的灵感来自于 ABC、Haskell、Java、Lisp、Icon 和 Perl 编程语言。Python 是一种高级的、通用的、多平台的解释性语言，是一种极简语言，由世界各地的一大群志愿者维护。

官方网站是 https://python.org

## PyQt6 version
`QT_VERSION_STR` 可以显示 Qt 的版本信息，`PYQT_VERSION_STR` 可以显示 PyQt 的版本信息

``` python
# file: version.py
#!/usr/bin/python

from PyQt6.QtCore import QT_VERSION_STR
from PyQt6.QtCore import PYQT_VERSION_STR

print(QT_VERSION_STR)
print(PYQT_VERSION_STR)
```
运行这个脚本可以显示 QT 和 PyQt 的版本。

``` sh
$ ./version.py 
6.0.2
6.0.3
```
这个章节介绍的是 PyQt 的工具类库。