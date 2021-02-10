# 控件\(1\)

控件就像是应用这座房子的一块块砖。PyQt5有很多的控件，比如按钮，单选框，滑动条，复选框等等。在本章，我们将介绍一些很有用的控件：`QCheckBox`，`ToggleButton`，`QSlider`，`QProgressBar`和`QCalendarWidget`。

## QCheckBox

`QCheckBox`组件有俩状态：开和关。通常跟标签一起使用，用在激活和关闭一些选项的场景。

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

"""
ZetCode PyQt5 tutorial 

In this example, a QCheckBox widget
is used to toggle the title of a window.

Author: Jan Bodnar
Website: zetcode.com 
Last edited: August 2017
"""

from PyQt5.QtWidgets import QWidget, QCheckBox, QApplication
from PyQt5.QtCore import Qt
import sys

class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):      

        cb = QCheckBox('Show title', self)
        cb.move(20, 20)
        cb.toggle()
        cb.stateChanged.connect(self.changeTitle)

        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('QCheckBox')
        self.show()


    def changeTitle(self, state):

        if state == Qt.Checked:
            self.setWindowTitle('QCheckBox')
        else:
            self.setWindowTitle(' ')


if __name__ == '__main__':

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())
```

这个例子中，有一个能切换窗口标题的单选框。

```text
cb = QCheckBox('Show title', self)
```

这个是`QCheckBox`的构造器。

```text
cb.toggle()
```

要设置窗口标题，我们就要检查单选框的状态。默认情况下，窗口没有标题，单选框未选中。

```text
cb.stateChanged.connect(self.changeTitle)
```

把`changeTitle()`方法和`stateChanged`信号关联起来。这样，`changeTitle()`就能切换窗口标题了。

```text
def changeTitle(self, state):

    if state == Qt.Checked:
        self.setWindowTitle('QCheckBox')
    else:
        self.setWindowTitle('')
```

控件的状态是由`changeTitle()`方法控制的，如果空间被选中，我们就给窗口添加一个标题，如果没被选中，就清空标题。

程序展示：

![QCheckBox](.gitbook/assets/6-qcheckbox.png)

## 切换按钮

切换按钮就是`QPushButton`的一种特殊模式。 它只有两种状态：按下和未按下。我们再点击的时候切换两种状态，有很多场景会使用到这个功能。

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

"""
ZetCode PyQt5 tutorial 

In this example, we create three toggle buttons.
They will control the background color of a 
QFrame. 

Author: Jan Bodnar
Website: zetcode.com 
Last edited: August 2017
"""

from PyQt5.QtWidgets import (QWidget, QPushButton, 
    QFrame, QApplication)
from PyQt5.QtGui import QColor
import sys

class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):      

        self.col = QColor(0, 0, 0)       

        redb = QPushButton('Red', self)
        redb.setCheckable(True)
        redb.move(10, 10)

        redb.clicked[bool].connect(self.setColor)

        greenb = QPushButton('Green', self)
        greenb.setCheckable(True)
        greenb.move(10, 60)

        greenb.clicked[bool].connect(self.setColor)

        blueb = QPushButton('Blue', self)
        blueb.setCheckable(True)
        blueb.move(10, 110)

        blueb.clicked[bool].connect(self.setColor)

        self.square = QFrame(self)
        self.square.setGeometry(150, 20, 100, 100)
        self.square.setStyleSheet("QWidget { background-color: %s }" %  
            self.col.name())

        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('Toggle button')
        self.show()


    def setColor(self, pressed):

        source = self.sender()

        if pressed:
            val = 255
        else: val = 0

        if source.text() == "Red":
            self.col.setRed(val)                
        elif source.text() == "Green":
            self.col.setGreen(val)             
        else:
            self.col.setBlue(val) 

        self.square.setStyleSheet("QFrame { background-color: %s }" %
            self.col.name())  


if __name__ == '__main__':

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())
```

我们创建了一个切换按钮和一个`QWidget`，并把`QWidget`的背景设置为黑色。点击不同的切换按钮，背景色会在红、绿、蓝之间切换（而且能看到颜色合成的效果，而不是单纯的颜色覆盖）。

```text
self.col = QColor(0, 0, 0)
```

设置颜色为黑色。

```text
redb = QPushButton('Red', self)
redb.setCheckable(True)
redb.move(10, 10)
```

创建一个`QPushButton`，然后调用它的`setCheckable()`的方法就把这个按钮编程了切换按钮。

```text
redb.clicked[bool].connect(self.setColor)
```

把点击信号和我们定义好的函数关联起来，这里是把点击事件转换成布尔值。

```text
source = self.sender()
```

获取被点击的按钮。

```text
if source.text() == "Red":
    self.col.setRed(val)
```

如果是标签为“red”的按钮被点击，就把颜色更改为预设好的对应颜色。

```text
self.square.setStyleSheet("QFrame { background-color: %s }" %
    self.col.name())
```

使用样式表（就是CSS的SS）改变背景色

程序展示：

![toggle button](.gitbook/assets/6-togglebutton.png)

## 滑块

`QSlider`是个有一个小滑块的组件，这个小滑块能拖着前后滑动，这个经常用于修改一些具有范围的数值，比文本框或者点击增加减少的文本框（spin box）方便多了。

本例用一个滑块和一个标签展示。标签为一个图片，滑块控制标签（的值）。

> 先准备四个分别表示静音、小音量、中音量、大音量的图标，文件名分别叫mute.png, min.png, med.png, max.png。

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

"""
ZetCode PyQt5 tutorial 

This example shows a QSlider widget.

Author: Jan Bodnar
Website: zetcode.com 
Last edited: August 2017
"""

from PyQt5.QtWidgets import (QWidget, QSlider, 
    QLabel, QApplication)
from PyQt5.QtCore import Qt
from PyQt5.QtGui import QPixmap
import sys

class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):      

        sld = QSlider(Qt.Horizontal, self)
        sld.setFocusPolicy(Qt.NoFocus)
        sld.setGeometry(30, 40, 100, 30)
        sld.valueChanged[int].connect(self.changeValue)

        self.label = QLabel(self)
        self.label.setPixmap(QPixmap('mute.png'))
        self.label.setGeometry(160, 40, 80, 30)

        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('QSlider')
        self.show()


    def changeValue(self, value):

        if value == 0:
            self.label.setPixmap(QPixmap('mute.png'))
        elif value > 0 and value <= 30:
            self.label.setPixmap(QPixmap('min.png'))
        elif value > 30 and value < 80:
            self.label.setPixmap(QPixmap('med.png'))
        else:
            self.label.setPixmap(QPixmap('max.png'))


if __name__ == '__main__':

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())
```

这里是模拟的音量控制器。拖动滑块，能改变标签位置的图片。

```text
sld = QSlider(Qt.Horizontal, self)
```

创建一个水平的`QSlider`。

```text
self.label = QLabel(self)
self.label.setPixmap(QPixmap('mute.png'))
```

创建一个`QLabel`组件并给它设置一个静音图标。

```text
sld.valueChanged[int].connect(self.changeValue)
```

把`valueChanged`信号跟`changeValue()`方法关联起来。

```text
if value == 0:
    self.label.setPixmap(QPixmap('mute.png'))
...
```

根据音量值的大小更换标签位置的图片。这段代码是：如果音量为0，就把图片换成 mute.png。

程序展示：

![QSlider widget](.gitbook/assets/6-qslider.png)

## 进度条

进度条是用来展示任务进度的（我也不想这样说话）。它的滚动能让用户了解到任务的进度。`QProgressBar`组件提供了水平和垂直两种进度条，进度条可以设置最大值和最小值，默认情况是0~99。

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

"""
ZetCode PyQt5 tutorial 

This example shows a QProgressBar widget.

Author: Jan Bodnar
Website: zetcode.com 
Last edited: August 2017
"""

from PyQt5.QtWidgets import (QWidget, QProgressBar, 
    QPushButton, QApplication)
from PyQt5.QtCore import QBasicTimer
import sys

class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):      

        self.pbar = QProgressBar(self)
        self.pbar.setGeometry(30, 40, 200, 25)

        self.btn = QPushButton('Start', self)
        self.btn.move(40, 80)
        self.btn.clicked.connect(self.doAction)

        self.timer = QBasicTimer()
        self.step = 0

        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('QProgressBar')
        self.show()


    def timerEvent(self, e):

        if self.step >= 100:
            self.timer.stop()
            self.btn.setText('Finished')
            return

        self.step = self.step + 1
        self.pbar.setValue(self.step)


    def doAction(self):

        if self.timer.isActive():
            self.timer.stop()
            self.btn.setText('Start')
        else:
            self.timer.start(100, self)
            self.btn.setText('Stop')


if __name__ == '__main__':

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())
```

我们创建了一个水平的进度条和一个按钮，这个按钮控制进度条的开始和停止。

```text
self.pbar = QProgressBar(self)
```

新建一个`QProgressBar`构造器。

```text
self.timer = QtCore.QBasicTimer()
```

用时间控制进度条。

```text
self.timer.start(100, self)
```

调用`start()`方法加载一个时间事件。这个方法有两个参数：过期时间和事件接收者。

```text
def timerEvent(self, e):

    if self.step >= 100:

        self.timer.stop()
        self.btn.setText('Finished')
        return

    self.step = self.step + 1
    self.pbar.setValue(self.step)
```

每个`QObject`和又它继承而来的对象都有一个`timerEvent()`事件处理函数。为了触发事件，我们重载了这个方法。

```text
def doAction(self):

    if self.timer.isActive():
        self.timer.stop()
        self.btn.setText('Start')

    else:
        self.timer.start(100, self)
        self.btn.setText('Stop')
```

里面的`doAction()`方法是用来控制开始和停止的。

程序展示：

![QProgressBar](.gitbook/assets/6-qprogressbar.png)

## 日历

`QCalendarWidget`提供了基于月份的日历插件，十分简易而且直观。

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

"""
ZetCode PyQt5 tutorial 

This example shows a QCalendarWidget widget.

Author: Jan Bodnar
Website: zetcode.com 
Last edited: August 2017
"""

from PyQt5.QtWidgets import (QWidget, QCalendarWidget, 
    QLabel, QApplication, QVBoxLayout)
from PyQt5.QtCore import QDate
import sys

class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):      

        vbox = QVBoxLayout(self)

        cal = QCalendarWidget(self)
        cal.setGridVisible(True)
        cal.clicked[QDate].connect(self.showDate)

        vbox.addWidget(cal)

        self.lbl = QLabel(self)
        date = cal.selectedDate()
        self.lbl.setText(date.toString())

        vbox.addWidget(self.lbl)

        self.setLayout(vbox)

        self.setGeometry(300, 300, 350, 300)
        self.setWindowTitle('Calendar')
        self.show()


    def showDate(self, date):     

        self.lbl.setText(date.toString())


if __name__ == '__main__':

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())
```

这个例子有日期组件和标签组件组成，标签显示被选中的日期。

```text
cal = QCalendarWidget(self)
```

创建一个`QCalendarWidget`。

```text
cal.clicked[QDate].connect(self.showDate)
```

选择一个日期时，`QDate`的点击信号就触发了，把这个信号和我们自己定义的`showDate()`方法关联起来。

```text
def showDate(self, date):     

    self.lbl.setText(date.toString())
```

使用`selectedDate()`方法获取选中的日期，然后把日期对象转成字符串，在标签里面显示出来。

程序展示：

![calendar](.gitbook/assets/6-calendar.png)

