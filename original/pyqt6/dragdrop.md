# Drag and drop in PyQt6
*last modified May 15, 2021*

In this part of the PyQt6 tutorial, we cover drag & drop operations.

In computer graphical user interfaces, drag-and-drop is the action of (or support for the action of) clicking on a virtual object and dragging it to a different location or onto another virtual object. In general, it can be used to invoke many kinds of actions, or create various types of associations between two abstract objects.

Drag and drop is part of the graphical user interface. Drag and drop operations enable users to do complex things intuitively.

Usually, we can drag and drop two things: data or some graphical objects. If we drag an image from one application to another, we drag and drop binary data. If we drag a tab in Firefox and move it to another place, we drag and drop a graphical component.

## QDrag
QDrag provides support for MIME-based drag and drop data transfer. It handles most of the details of a drag and drop operation. The transferred data is contained in a QMimeData object.

## Simple drag and drop example in PyQt6
In the first example, we have a `QLineEdit` and a `QPushButton`. We drag plain text from the line edit widget and drop it onto the button widget. The button's label will change.

``` python
# file: simple.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This is a simple drag and
drop example.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtWidgets import (QPushButton, QWidget,
        QLineEdit, QApplication)


class Button(QPushButton):

    def __init__(self, title, parent):
        super().__init__(title, parent)

        self.setAcceptDrops(True)


    def dragEnterEvent(self, e):

        if e.mimeData().hasFormat('text/plain'):
            e.accept()
        else:
            e.ignore()


    def dropEvent(self, e):

        self.setText(e.mimeData().text())


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        edit = QLineEdit('', self)
        edit.setDragEnabled(True)
        edit.move(30, 65)

        button = Button("Button", self)
        button.move(190, 65)

        self.setWindowTitle('Simple drag and drop')
        self.setGeometry(300, 300, 300, 150)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    ex.show()
    app.exec()


if __name__ == '__main__':
    main()
```
The example presents a simple drag & drop operation.

``` python
class Button(QPushButton):

    def __init__(self, title, parent):
        super().__init__(title, parent)

        ...
```
In order to drop text on the `QPushButton` widget, we must reimplement some methods. Therefore, we create our own Button class which inherits from the QPushButton class.

``` python
self.setAcceptDrops(True)
```
We enable drop events for the widget with setAcceptDrops.

```python
def dragEnterEvent(self, e):

    if e.mimeData().hasFormat('text/plain'):
        e.accept()
    else:
        e.ignore()
```
First, we reimplement the dragEnterEvent method. We inform about the data type that we accept. In our case it is plain text.

```python
def dropEvent(self, e):

    self.setText(e.mimeData().text())
```
By reimplementing the dropEvent method we define what happes at the drop event. Here we change the text of the button widget.

```python
edit = QLineEdit('', self)
edit.setDragEnabled(True)
```
The QLineEdit widget has a built-in support for drag operations. All we need to do is to call the setDragEnabled method to activate it.

![Simple drag and drop](./images/dragdrop.png)

Figure: Simple drag and drop

## Drag and drop a button widget
The following example demonstrates how to drag and drop a button widget.

``` python
# file: drag_button.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this program, we can press on a button with a left mouse
click or drag and drop the button with  the right mouse click.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtCore import Qt, QMimeData
from PyQt6.QtGui import QDrag
from PyQt6.QtWidgets import QPushButton, QWidget, QApplication


class Button(QPushButton):

    def __init__(self, title, parent):
        super().__init__(title, parent)


    def mouseMoveEvent(self, e):

        if e.buttons() != Qt.MouseButtons.RightButton:
            return

        mimeData = QMimeData()

        drag = QDrag(self)
        drag.setMimeData(mimeData)

        drag.setHotSpot(e.position().toPoint() - self.rect().topLeft())

        dropAction = drag.exec(Qt.DropActions.MoveAction)


    def mousePressEvent(self, e):

        super().mousePressEvent(e)

        if e.button() == Qt.MouseButtons.LeftButton:
            print('press')


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setAcceptDrops(True)

        self.button = Button('Button', self)
        self.button.move(100, 65)

        self.setWindowTitle('Click or Move')
        self.setGeometry(300, 300, 550, 450)


    def dragEnterEvent(self, e):

        e.accept()


    def dropEvent(self, e):

        position = e.position()
        self.button.move(position.toPoint())

        e.setDropAction(Qt.DropActions.MoveAction)
        e.accept()


def main():
    
    app = QApplication(sys.argv)
    ex = Example()
    ex.show()
    app.exec()


if __name__ == '__main__':
    main()
```

In our code example, we have a `QPushButton` on the window. If we click on the button with a left mouse button, the 'press' message is printed to the console. By right clicking and moving the button, we perform a drag and drop operation on the button widget.

``` python
class Button(QPushButton):

    def __init__(self, title, parent):
        super().__init__(title, parent)
```
We create a Button class which derives from the `QPushButton`. We also reimplement two methods of the `QPushButton`: the mouseMoveEvent and the mousePressEvent. The mouseMoveEvent method is the place where the drag and drop operation begins.

``` python
if e.buttons() != Qt.MouseButtons.RightButton:
    return
```
Here we decide that we can perform drag and drop only with a right mouse button. The left mouse button is reserved for clicking on the button.

``` python
drag = QDrag(self)
drag.setMimeData(mimeData)

drag.setHotSpot(e.position().toPoint() - self.rect().topLeft())
```
The `QDrag` object is created. The class provides support for MIME-based drag and drop data transfer.

``` python
dropAction = drag.exec(Qt.DropActions.MoveAction)
```
The exec method of the drag object starts the drag and drop operation.

``` python
def mousePressEvent(self, e):

    super().mousePressEvent(e)

    if e.button() == Qt.MouseButtons.LeftButton:
        print('press')
```
We print 'press' to the console if we left click on the button with the mouse. Notice that we call mousePressEvent method on the parent as well. Otherwise, we would not see the button being pushed.

``` python
position = e.pos()
self.button.move(position)
```
In the dropEvent method we specify what happens after we release the mouse button and finish the drop operation. In our case, we find out the current mouse pointer position and move the button accordingly.

``` python
e.setDropAction(Qt.MoveAction)
e.accept()
```
We specify the type of the drop action with `setDropAction`. In our case it is a move action.

This part of the PyQt6 tutorial was dedicated to drag and drop operations.