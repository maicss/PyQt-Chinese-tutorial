# PyQt6 的菜单和工具栏
*最后更新于 20201.04.24*

在这部分教程中，我们创建了一个状态栏、菜单栏和工具栏。菜单是位于菜单栏中的一组命令。工具栏有一些按钮和应用程序中的一些常用命令。状态栏显示状态信息，通常位于应用程序窗口的底部。

## PyQt6 QMainWindow
`QMainWindow` 类提供了主程序窗口。在这里可以创建一个具有状态栏、工具栏和菜单栏的经典应用程序框架。
## PyQt6 状态栏
状态栏是显示状态信息的小部件。

``` python
# file: statusbar.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a statusbar.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QApplication


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.statusBar().showMessage('Ready')

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Statusbar')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
使用 `QMainWindow` 创建状态栏。
``` python
self.statusBar().showMessage('Ready')
```
使用 `QtGui.QMainWindow` 方法创建状态栏，该方法的创建了一个状态栏，并返回statusbar对象，再调用 `showMessage` 方法在状态栏上显示一条消息。

## PyQt6 简单菜单
菜单栏在GUI应用程序中很常见，它是位于各种菜单中的一组命令。(Mac OS 对菜单栏的处理是不同的，要得到类似的结果，我们可以添加下面这行: `menubar.setNativeMenuBar(False)`
``` python
# file: simple_menu.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a menubar. The
menubar has one menu with an exit action.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QApplication
from PyQt6.QtGui import QIcon, QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        exitAct = QAction(QIcon('exit.png'), '&Exit', self)
        exitAct.setShortcut('Ctrl+Q')
        exitAct.setStatusTip('Exit application')
        exitAct.triggered.connect(QApplication.instance().quit)

        self.statusBar()

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(exitAct)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Simple menu')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
上门的示例中，创建了有一个菜单的菜单栏。这个菜单命令是终止应用，也绑定了快捷键 `Ctrl+Q`。示例中也创建了一个状态栏。
``` python
exitAct = QAction(QIcon('exit.png'), '&Exit', self)
exitAct.setShortcut('Ctrl+Q')
exitAct.setStatusTip('Exit application')
```
`QAction` 是行为抽象类，包括菜单栏，工具栏，或自定义键盘快捷方式。在上面的三行中，创建了一个带有特定图标和 'Exit' 标签的行为。此外，还为该行为定义了一个快捷方式。第三行创建一个状态提示，当我们将鼠标指针悬停在菜单项上时，状态栏中就会显示这个提示。
``` python
exitAct.triggered.connect(QApplication.instance().quit)
```
当选择指定的行为时，触发了一个信号，这个信号连接了 `QApplication` 组件的退出操作，这会终止这个应用程序。
``` python
menubar = self.menuBar()
fileMenu = menubar.addMenu('&File')
fileMenu.addAction(exitAction)
```
`menuBar` 方法创建了一个菜单栏，然后使用 `addMenu` 创建一个文件菜单，使用 `addAction` 创建一个行为。

## PyQt6 子菜单
子菜单是位于菜单里的菜单。
``` python
submenu.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a submenu.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QMenu, QApplication
from PyQt6.QtGui import QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('File')

        impMenu = QMenu('Import', self)
        impAct = QAction('Import mail', self)
        impMenu.addAction(impAct)

        newAct = QAction('New', self)

        fileMenu.addAction(newAct)
        fileMenu.addMenu(impMenu)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Submenu')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
本例中，有两个菜单项，一个在 File 菜单里，一个在 File 的 Import 子菜单里。

``` python
impMenu = QMenu('Import', self)
```

使用 `QMenu` 创建一个新菜单。

``` python
impAct = QAction('Import mail', self)
impMenu.addAction(impAct)
```

使用`addAction` 给子菜单添加行为.

![Submenu](./images/submenu.png)

图示：子菜单

## PyQt6 勾选菜单
下面的示例中，创建了一个可以勾选的菜单。

``` python
# file: check_menu.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a checkable menu.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QApplication
from PyQt6.QtGui import QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.statusbar = self.statusBar()
        self.statusbar.showMessage('Ready')

        menubar = self.menuBar()
        viewMenu = menubar.addMenu('View')

        viewStatAct = QAction('View statusbar', self, checkable=True)
        viewStatAct.setStatusTip('View statusbar')
        viewStatAct.setChecked(True)
        viewStatAct.triggered.connect(self.toggleMenu)

        viewMenu.addAction(viewStatAct)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Check menu')
        self.show()


    def toggleMenu(self, state):

        if state:
            self.statusbar.show()
        else:
            self.statusbar.hide()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
创建只有一个行为的 View 菜单。这个行为用来展现或者隐藏状态栏，如果状态栏可见，菜单是勾选的状态。

``` python
viewStatAct = QAction('View statusbar', self, checkable=True)
```
使用 `checkable` 参数创建一个可以勾选的菜单。

``` python
viewStatAct.setChecked(True)
```
因为状态栏默认可见，所以使用 `setChecked` 方法勾选菜单。

``` python
def toggleMenu(self, state):

    if state:
        self.statusbar.show()
    else:
        self.statusbar.hide()
```
根据行为的状态，设置状态栏的状态。

![Check menu](./images/checkmenu.png)

图书：勾选菜单
## PyQt6 上下文菜单
上下文菜单，也称为弹出菜单，是在某些上下文下出现的命令列表。例如，在 Opera 浏览器中，在网页上按下鼠标右键，我们会得到一个上下文菜单，这个菜单上，可以重新加载页面、返回或查看页面源代码。如果右击工具栏，会得到另一个用于管理工具栏的上下文菜单。

``` python
# file: context_menu.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a context menu.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QMenu, QApplication


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Context menu')
        self.show()


    def contextMenuEvent(self, event):

        cmenu = QMenu(self)

        newAct = cmenu.addAction("New")
        openAct = cmenu.addAction("Open")
        quitAct = cmenu.addAction("Quit")
        action = cmenu.exec(self.mapToGlobal(event.pos()))

        if action == quitAct:
            QApplication.instance().quit()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
重新实现 `contextMenuEvent` 方法，调出一个上下文菜单。

``` python
action = cmenu.exec(self.mapToGlobal(event.pos()))
```
使用 `exec` 方法调出上下文菜单，通过鼠标事件对象获得鼠标坐标点，再调用 `mapToGlobal` 方法把组件的坐标设置成全局的屏幕坐标。
``` python
if action == quitAct:
    QApplication.instance().quit()
```
如果上下文菜单触发的动作是退出动作，就终止程序。

## PyQt6 工具栏
菜单包含了一个应用程序里所有需要使用到的命令，工具栏则是放置常用命令的地方。

``` python 
# file: toolbar.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a toolbar.
The toolbar has one action, which
terminates the application, if triggered.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow,  QApplication
from PyQt6.QtGui import QIcon, QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        exitAct = QAction(QIcon('exit24.png'), 'Exit', self)
        exitAct.setShortcut('Ctrl+Q')
        exitAct.triggered.connect(QApplication.instance().quit)

        self.toolbar = self.addToolBar('Exit')
        self.toolbar.addAction(exitAct)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Toolbar')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
上面的示例中创建了一个简单的状态栏，只有一个行为，关闭应用。

``` python
exitAct = QAction(QIcon('exit24.png'), 'Exit', self)
exitAct.setShortcut('Ctrl+Q')
exitAct.triggered.connect(QApplication.instance().quit)
```
和上面菜单栏示例相似，创建一个行为对象，对象有标签，图标和快捷键。然后把 `QApplication` 的退出方法和行为发出的信号绑定。

``` python
self.toolbar = self.addToolBar('Exit')
self.toolbar.addAction(exitAction)
```
使用 `addToolBar` 方法创建工具栏，然后使用 `addAction` 方法添加行为。

![Toolbar](./images/toolbar.png)

图示：工具栏
## PyQt6 主窗口
这是本章最后一个示例，这里创建一个菜单栏，一个工具栏和一个状态栏，并增加一个中心布局组件。

``` python
# file: main_window.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a skeleton of
a classic GUI application with a menubar,
toolbar, statusbar, and a central widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QTextEdit, QApplication
from PyQt6.QtGui import QIcon, QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        textEdit = QTextEdit()
        self.setCentralWidget(textEdit)

        exitAct = QAction(QIcon('exit24.png'), 'Exit', self)
        exitAct.setShortcut('Ctrl+Q')
        exitAct.setStatusTip('Exit application')
        exitAct.triggered.connect(self.close)

        self.statusBar()

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(exitAct)

        toolbar = self.addToolBar('Exit')
        toolbar.addAction(exitAct)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Main window')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
示例展示了一个经典的 GUI 应用的布局，有一个菜单栏，一个工具栏和一个状态栏。
``` python
textEdit = QTextEdit()
self.setCentralWidget(textEdit)
```
这里创建了一个文本编辑器组件，并把它设置到 `QMainWindow` 的中央。居中布局组件撑满了所有空白的部分。

![Main window](./images/mainwindow.png)

图示：主窗口

本部分 PyQt6 教程，我们展示了菜单，工具栏，状态栏和主程序窗口。