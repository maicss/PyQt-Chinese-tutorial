# Introduction to PyQt6
*last modified April 22, 2021*

This is an introductory PyQt6 tutorial. The purpose of this tutorial is to get you started with the PyQt6 toolkit.

About PyQt6
PyQt6 is a set of Python bindings for Qt5 application framework from Digia. Qt library is one of the most powerful GUI libraries. The official home site for PyQt6 is www.riverbankcomputing.co.uk/news. PyQt6 is developed by Riverbank Computing.

PyQt6 is implemented as a set of Python modules. It is a multiplatform toolkit which runs on all major operating systems, including Unix, Windows, and Mac OS. PyQt6 is dual licensed; developers can choose between a GPL and a commercial license.

PyQt6 installation
```sh
$ pip install PyQt6
```
We can install PyQt6 with the `pip` tool.

PyQt6 modules
PyQt6's classes are divided into several modules, including the following:

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

The `QtCore` module contains the core non-GUI functionality. This module is used for working with time, files and directories, various data types, streams, URLs, mime types, threads or processes. The `QtGui` contains classes for windowing system integration, event handling, 2D graphics, basic imaging, fonts and text. The `QtWidgets` module contains classes that provide a set of UI elements to create classic desktop-style user interfaces.

The `QtDBus` contains classes to support IPC using the D-Bus protocol.The `QtNetwork` module contains the classes for network programming. These classes facilitate the coding of TCP/IP and UDP clients and servers by making the network programming easier and more portable. The `QtHelp` contains classes for creating and viewing searchable documentation.

The `QtXml` contains classes for working with XML files. This module provides implementation for both SAX and DOM APIs. The `QtSvg` module provides classes for displaying the contents of SVG files. Scalable Vector Graphics (SVG) is a language for describing two-dimensional graphics and graphical applications in XML. The `QtSql` module provides classes for working with databases. The `QtTest` contains functions that enable unit testing of PyQt6 applications.

Python
Python is a general-purpose, dynamic, object-oriented programming language. The design purpose of the Python language emphasizes programmer productivity and code readability. It was first released in 1991. Python was inspired by ABC, Haskell, Java, Lisp, Icon, and Perl programming languages. Python is a high-level, general purpose, multiplatform, interpreted language. Python is a minimalistic language. Python is maintained by a large group of volunteers worldwide.

The official web site for the Python programming language is python.org

PyQt6 version
The `QT_VERSION_STR` provides the version of Qt and the `PYQT_VERSION_STR` version of PyQt6.

``` python
# file: version.py
#!/usr/bin/python

from PyQt6.QtCore import QT_VERSION_STR
from PyQt6.QtCore import PYQT_VERSION_STR

print(QT_VERSION_STR)
print(PYQT_VERSION_STR)
```
We print the version of Qt library and PyQt6 module.

``` sh
$ ./version.py 
6.0.2
6.0.3
```
This chapter was an introduction to PyQt6 toolkit.