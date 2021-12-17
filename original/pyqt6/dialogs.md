# Dialogs in PyQt6
*last modified April 30, 2021*

A dialog is defined as a conversation between two or more persons. In a computer application a dialog is a window which is used to "talk" to the application. Dialogs are used for things such as getting data from users or changing application settings.

## PyQt6 QInputDialog
QInputDialog provides a simple convenience dialog to get a single value from the user. The input value can be a string, a number, or an item from a list.

``` python
# file: input_dialog.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we receive data from
a QInputDialog dialog.

Aauthor: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QPushButton, QLineEdit,
        QInputDialog, QApplication)
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.btn = QPushButton('Dialog', self)
        self.btn.move(20, 20)
        self.btn.clicked.connect(self.showDialog)

        self.le = QLineEdit(self)
        self.le.move(130, 22)

        self.setGeometry(300, 300, 450, 350)
        self.setWindowTitle('Input dialog')
        self.show()


    def showDialog(self):

        text, ok = QInputDialog.getText(self, 'Input Dialog',
                                        'Enter your name:')

        if ok:
            self.le.setText(str(text))


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
The example has a button and a line edit widget. The button shows the input dialog for getting text values. The entered text will be displayed in the line edit widget.

``` python
text, ok = QInputDialog.getText(self, 'Input Dialog',
    'Enter your name:')
```
This line displays the input dialog. The first string is a dialog title, the second one is a message within the dialog. The dialog returns the entered text and a boolean value. If we click the Ok button, the boolean value is true.

``` python
if ok:
    self.le.setText(str(text))
```
The text that we have received from the dialog is set to the line edit widget with `setText()`.

![Input dialog](./images/inputdialog.png)

Figure: Input dialog

## PyQt6 QColorDialog
`QColorDialog` provides a dialog widget for selecting colour values.

``` python
# file: color_dialog.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we select a color value
from the QColorDialog and change the background
color of a QFrame widget.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QPushButton, QFrame,
        QColorDialog, QApplication)
from PyQt6.QtGui import QColor
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        col = QColor(0, 0, 0)

        self.btn = QPushButton('Dialog', self)
        self.btn.move(20, 20)

        self.btn.clicked.connect(self.showDialog)

        self.frm = QFrame(self)
        self.frm.setStyleSheet("QWidget { background-color: %s }"
                               % col.name())
        self.frm.setGeometry(130, 22, 200, 200)

        self.setGeometry(300, 300, 450, 350)
        self.setWindowTitle('Color dialog')
        self.show()


    def showDialog(self):

        col = QColorDialog.getColor()

        if col.isValid():

            self.frm.setStyleSheet("QWidget { background-color: %s }" 
                                   % col.name())


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
The application example shows a push button and a `QFrame`. The widget background is set to black colour. Using the `QColorDialog`, we can change its background.
``` python
col = QColor(0, 0, 0)
```
This is an initial colour of the QFrame background.
``` python
col = QColorDialog.getColor()
```
This line pops up the QColorDialog.
``` python
if col.isValid():

    self.frm.setStyleSheet("QWidget { background-color: %s }" 
                           % col.name())
```
We check if the colour is valid. If we click on the Cancel button, no valid colour is returned. If the colour is valid, we change the background colour using style sheets.

## PyQt6 QFontDialog
`QFontDialog` is a dialog widget for selecting a font.

``` python
# file: font_dialog.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we select a font name
and change the font of a label.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QVBoxLayout, QPushButton,
        QSizePolicy, QLabel, QFontDialog, QApplication)
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        vbox = QVBoxLayout()

        btn = QPushButton('Dialog', self)
        btn.setSizePolicy(QSizePolicy.Policy.Fixed, QSizePolicy.Policy.Fixed)
        btn.move(20, 20)

        vbox.addWidget(btn)

        btn.clicked.connect(self.showDialog)

        self.lbl = QLabel('Knowledge only matters', self)
        self.lbl.move(130, 20)

        vbox.addWidget(self.lbl)
        self.setLayout(vbox)

        self.setGeometry(300, 300, 450, 350)
        self.setWindowTitle('Font dialog')
        self.show()


    def showDialog(self):

        font, ok = QFontDialog.getFont()

        if ok:
            self.lbl.setFont(font)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
In our example, we have a button and a label. With the `QFontDialog`, we change the font of the label.
``` python
font, ok = QFontDialog.getFont()
```
Here we pop up the font dialog. The getFont method returns the font name and the ok parameter. It is equal to True if the user clicked Ok; otherwise it is False.
``` python
if ok:
    self.label.setFont(font)
```
If we clicked Ok, the font of the label is changed with setFont.

## PyQt6 QFileDialog

`QFileDialog` is a dialog that allows users to select files or directories. The files can be selected for both opening and saving.

``` python
# file: file_dialog.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we select a file with a
QFileDialog and display its contents
in a QTextEdit.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QMainWindow, QTextEdit,
        QFileDialog, QApplication)
from PyQt6.QtGui import QIcon, QAction
from pathlib import Path
import sys


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.textEdit = QTextEdit()
        self.setCentralWidget(self.textEdit)
        self.statusBar()

        openFile = QAction(QIcon('open.png'), 'Open', self)
        openFile.setShortcut('Ctrl+O')
        openFile.setStatusTip('Open new File')
        openFile.triggered.connect(self.showDialog)

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(openFile)

        self.setGeometry(300, 300, 550, 450)
        self.setWindowTitle('File dialog')
        self.show()


    def showDialog(self):

        home_dir = str(Path.home())
        fname = QFileDialog.getOpenFileName(self, 'Open file', home_dir)

        if fname[0]:

            f = open(fname[0], 'r')

            with f:

                data = f.read()
                self.textEdit.setText(data)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```
The example shows a menubar, centrally set text edit widget, and a statusbar. The menu item shows the `QFileDialog` which is used to select a file. The contents of the file are loaded into the text edit widget.

``` python
class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()
```
The example is based on the `QMainWindow` widget because we centrally set a text edit widget.
``` python
home_dir = str(Path.home())
fname = QFileDialog.getOpenFileName(self, 'Open file', home_dir)
```
We pop up the QFileDialog. The first string in the getOpenFileName method is the caption. The second string specifies the dialog working directory. We use the path module to determine the user's home directory. By default, the file filter is set to All files (*).

``` python
if fname[0]:

    f = open(fname[0], 'r')

    with f:

        data = f.read()
        self.textEdit.setText(data)
```
The selected file name is read and the contents of the file are set to the text edit widget.

In this part of the PyQt6 tutorial, we worked with dialogs.