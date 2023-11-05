# PyQt6 组件（二）
*最后更新于 2021.05.05*

本章继续介绍 PyQt6 组件，包含了`QPixmap`, `QLineEdit`, `QSplitter` 和 `QComboBox`。

## PyQt6 QPixmap
`QPixmap` 是用于处理图像的小组件，为显示图像进行了优化。下面是使用 `QPixmap` 渲染图像的示例。

``` python
# file: pixmap.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we display an image
on the window.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QHBoxLayout,
        QLabel, QApplication)
from PyQt6.QtGui import QPixmap
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        hbox = QHBoxLayout(self)
        pixmap = QPixmap('sid.jpg')

        lbl = QLabel(self)
        lbl.setPixmap(pixmap)

        hbox.addWidget(lbl)
        self.setLayout(hbox)

        self.move(300, 200)
        self.setWindowTitle('Sid')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
在窗口里展示了一个图片。
``` python
pixmap = QPixmap('sid.jpg')
```
用文件名作为参数创建一个 `QPixmap` 对象。
``` python
lbl = QLabel(self)
lbl.setPixmap(pixmap)
```
然后把 `pixmap` 放到 `QLabel` 组件里。

## PyQt6 QLineEdit
QLineEdit是一个可以输入单行文本的组件，它有撤消和重做、剪切和粘贴以及拖放功能。

``` python
# file: line_edit.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows text which
is entered in a QLineEdit
in a QLabel widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import (QWidget, QLabel,
        QLineEdit, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.lbl = QLabel(self)
        qle = QLineEdit(self)

        qle.move(60, 100)
        self.lbl.move(60, 40)

        qle.textChanged[str].connect(self.onChanged)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('QLineEdit')
        self.show()


    def onChanged(self, text):

        self.lbl.setText(text)
        self.lbl.adjustSize()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
本例中有一个 `QLineEdit` 组件和一个标签，在编辑器里输入的文本会立即显示在标签里。
``` python
qle = QLineEdit(self)
```
创建一个 `QLineEdit` 组件。
``` python
qle.textChanged[str].connect(self.onChanged)
```
如果编辑器的文本发生了变化，就调用 `onChanged` 方法。
``` python
def onChanged(self, text):
    
    self.lbl.setText(text)
    self.lbl.adjustSize()
```
在 `onChanged` 里，把输入的文本设置到标签组件里，同时使用 `adjustSize` 方法调整文字显示。

![QLineEdit](./images/qlineedit.png)

图示：QLineEdit

## PyQt6 QSplitter
`QSplitter` 允许用户通过拖动子部件之间的边界来控制子部件的大小。下面是被两个分割条分开的三个`QFrame` 组件的示例。
``` python
# file: splitter.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows
how to use QSplitter widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtCore import Qt
from PyQt6.QtWidgets import (QWidget, QHBoxLayout, QFrame,
        QSplitter, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        hbox = QHBoxLayout(self)

        topleft = QFrame(self)
        topleft.setFrameShape(QFrame.Shape.StyledPanel)

        topright = QFrame(self)
        topright.setFrameShape(QFrame.Shape.StyledPanel)

        bottom = QFrame(self)
        bottom.setFrameShape(QFrame.Shape.StyledPanel)

        splitter1 = QSplitter(Qt.Orientation.Horizontal)
        splitter1.addWidget(topleft)
        splitter1.addWidget(topright)

        splitter2 = QSplitter(Qt.Orientation.Vertical)
        splitter2.addWidget(splitter1)
        splitter2.addWidget(bottom)

        hbox.addWidget(splitter2)
        self.setLayout(hbox)

        self.setGeometry(300, 300, 450, 400)
        self.setWindowTitle('QSplitter')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
这里有三个 QFrame 组件和连个 QSplitter 组件，注意，在某些主题里，分割条可能不容易看到。
``` python
topleft = QFrame(self)
topleft.setFrameShape(QFrame.Shape.StyledPanel)
```
给框架组件设置一些样式，这样更容易看清楚边界。
``` python
splitter1 = QSplitter(Qt.Orientation.Horizontal)
splitter1.addWidget(topleft)
splitter1.addWidget(topright)
```
创建一个有俩框架组件的 `QSplitter` 组件。
``` python
splitter2 = QSplitter(Qt.Orientation.Vertical)
splitter2.addWidget(splitter1)
```
再添加一个分割条和一个框架组件。

![QSplitter widget](./images/qsplitter.png)

图示：分割条组件

## PyQt6 QComboBox
`QComboBox` 是下拉选框组件，能让用户在一系列选项中进行选择。

``` python
# file: combobox.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows how to use
a QComboBox widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtWidgets import (QWidget, QLabel,
        QComboBox, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.lbl = QLabel('Ubuntu', self)

        combo = QComboBox(self)

        combo.addItem('Ubuntu')
        combo.addItem('Mandriva')
        combo.addItem('Fedora')
        combo.addItem('Arch')
        combo.addItem('Gentoo')

        combo.move(50, 50)
        self.lbl.move(50, 150)

        combo.textActivated[str].connect(self.onActivated)

        self.setGeometry(300, 300, 450, 400)
        self.setWindowTitle('QComboBox')
        self.show()


    def onActivated(self, text):

        self.lbl.setText(text)
        self.lbl.adjustSize()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
示例中有一个 `QComboBox` 和一个 `QLabel`。下拉选框有一个包含五个选项的列表，是Linux发行版的名称。标签组件显示组合框中选择的选项。

``` python
combo = QComboBox(self)

combo.addItem('Ubuntu')
combo.addItem('Mandriva')
combo.addItem('Fedora')
combo.addItem('Arch')
combo.addItem('Gentoo')
```
创建有五个选项的 `QComboBox` 组件。
``` python
combo.textActivated[str].connect(self.onActivated)
```
如果选择了一个选项，就调用 `onActivated` 方法。
``` python
def onActivated(self, text):
  
    self.lbl.setText(text)
    self.lbl.adjustSize()
```
在这个方法里，设置选中的文本到标签组件里，然后调整标签组件大小。

![QComboBox](./images/qcombobox.png)

图示：QComboBox
