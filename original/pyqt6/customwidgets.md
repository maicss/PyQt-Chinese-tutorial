# Custom widgets in PyQt6
*last modified May 17, 2021*

PyQt6 has a rich set of widgets. However, no toolkit can provide all widgets that programmers might need in their applications. Toolkits usually provide only the most common widgets like buttons, text widgets, or sliders. If there is a need for a more specialised widget, we must create it ourselves.

Custom widgets are created by using the drawing tools provided by the toolkit. There are two basic possibilities: a programmer can modify or enhance an existing widget or he can create a custom widget from scratch.

## PyQt6 burning widget
This is a widget that we can see in Nero, K3B, or other CD/DVD burning software.

``` python
# file: burning_widget.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we create a custom widget.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QSlider, QApplication,
        QHBoxLayout, QVBoxLayout)
from PyQt6.QtCore import QObject, Qt, pyqtSignal
from PyQt6.QtGui import QPainter, QFont, QColor, QPen
import sys


class Communicate(QObject):
    updateBW = pyqtSignal(int)


class BurningWidget(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setMinimumSize(1, 30)
        self.value = 75
        self.num = [75, 150, 225, 300, 375, 450, 525, 600, 675]


    def setValue(self, value):

        self.value = value


    def paintEvent(self, e):

        qp = QPainter()
        qp.begin(self)
        self.drawWidget(qp)
        qp.end()


    def drawWidget(self, qp):

        MAX_CAPACITY = 700
        OVER_CAPACITY = 750

        font = QFont('Serif', 7, QFont.Weight.Light)
        qp.setFont(font)

        size = self.size()
        w = size.width()
        h = size.height()

        step = int(round(w / 10))

        till = int(((w / OVER_CAPACITY) * self.value))
        full = int(((w / OVER_CAPACITY) * MAX_CAPACITY))

        if self.value >= MAX_CAPACITY:

            qp.setPen(QColor(255, 255, 255))
            qp.setBrush(QColor(255, 255, 184))
            qp.drawRect(0, 0, full, h)
            qp.setPen(QColor(255, 175, 175))
            qp.setBrush(QColor(255, 175, 175))
            qp.drawRect(full, 0, till - full, h)

        else:

            qp.setPen(QColor(255, 255, 255))
            qp.setBrush(QColor(255, 255, 184))
            qp.drawRect(0, 0, till, h)

        pen = QPen(QColor(20, 20, 20), 1,
                   Qt.PenStyle.SolidLine)

        qp.setPen(pen)
        qp.setBrush(Qt.BrushStyle.NoBrush)
        qp.drawRect(0, 0, w - 1, h - 1)

        j = 0

        for i in range(step, 10 * step, step):

            qp.drawLine(i, 0, i, 5)
            metrics = qp.fontMetrics()
            fw = metrics.horizontalAdvance(str(self.num[j]))

            x, y = int(i - fw/2), int(h / 2)
            qp.drawText(x, y, str(self.num[j]))
            j = j + 1


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        OVER_CAPACITY = 750

        sld = QSlider(Qt.Orientations.Horizontal, self)
        sld.setFocusPolicy(Qt.FocusPolicy.NoFocus)
        sld.setRange(1, OVER_CAPACITY)
        sld.setValue(75)
        sld.setGeometry(30, 40, 150, 30)

        self.c = Communicate()
        self.wid = BurningWidget()
        self.c.updateBW[int].connect(self.wid.setValue)

        sld.valueChanged[int].connect(self.changeValue)
        hbox = QHBoxLayout()
        hbox.addWidget(self.wid)
        vbox = QVBoxLayout()
        vbox.addStretch(1)
        vbox.addLayout(hbox)
        self.setLayout(vbox)

        self.setGeometry(300, 300, 390, 210)
        self.setWindowTitle('Burning widget')
        self.show()


    def changeValue(self, value):

        self.c.updateBW.emit(value)
        self.wid.repaint()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
In our example, we have a `QSlider` and a custom widget. The slider controls the custom widget. This widget shows graphically the total capacity of a medium and the free space available to us. The minimum value of our custom widget is 1, the maximum is OVER_CAPACITY. If we reach value MAX_CAPACITY, we begin drawing in red colour. This normally indicates overburning.

The burning widget is placed at the bottom of the window. This is achieved using one `QHBoxLayout` and one `QVBoxLayout`.

```python
class BurningWidget(QWidget):

    def __init__(self):
        super().__init__()
```
The burning widget it based on the `QWidget` widget.

```python
self.setMinimumSize(1, 30)
```
We change the minimum size (height) of the widget. The default value is a bit small for us.
```python
font = QFont('Serif', 7, QFont.Weight.Light)
qp.setFont(font)
```
We use a smaller font than the default one. This better suits our needs.

```python
size = self.size()
w = size.width()
h = size.height()

step = int(round(w / 10))


till = int(((w / OVER_CAPACITY) * self.value))
full = int(((w / OVER_CAPACITY) * MAX_CAPACITY))
```
We draw the widget dynamically. The greater is the window, the greater is the burning widget and vice versa. That is why we must calculate the size of the widget onto which we draw the custom widget. The till parameter determines the total size to be drawn. This value comes from the slider widget. It is a proportion of the whole area. The full parameter determines the point where we begin to draw in red colour.

The actual drawing consists of three steps. We draw the yellow or the red and yellow rectangle. Then we draw the vertical lines which divide the widget into several parts. Finally, we draw the numbers which indicate the capacity of the medium.

``` python
metrics = qp.fontMetrics()
fw = metrics.horizontalAdvance(str(self.num[j]))

x, y = int(i - fw/2), int(h / 2)
qp.drawText(x, y, str(self.num[j]))
```
We use font metrics to draw the text. We must know the width of the text in order to center it around the vertical line.

``` python
def changeValue(self, value):

    self.c.updateBW.emit(value)
    self.wid.repaint()
```
When we move the slider, the changeValue method is called. Inside the method, we send a custom updateBW signal with a parameter. The parameter is the current value of the slider. The value is later used to calculate the capacity of the Burning widget to be drawn. The custom widget is then repainted.

![The burning widget](./images/burning.png)

Figure: The burning widget

In this part of the PyQt6 tutorial, we created a custom widget.