# 俄罗斯方块
*更新于 2021.05.05*

本章实现一个俄罗斯方块游戏。

## 简介
俄罗斯方块游戏是有史以来最受欢迎的电脑游戏之一。最初的游戏是由俄罗斯程序员 Alexey Pajitnov 在1985年设计并编写的。从那时起，《俄罗斯方块》便以多种形式出现在几乎所有平台上。

俄罗斯方块被称为掉落方块拼图游戏。在这款游戏中，我们有7种不同的形状，叫做砖块（tetrminoes）：S形、Z形、T形、L形、线形、反向L形和方形。每个形状都是由四个正方形组成的。这些形状从顶部掉落。《俄罗斯方块》游戏的目标是移动和旋转形状，尽可能的拼到一起，如果拼成一行，这一行会消失，这样就能得分，直到方块堆叠到顶部，游戏结束。

![Tetrominoes](./images/tetrominoes.png)

图示：Tetrominoes

PyQt6 目标是创建应用程序，有些其他的库的目标是创造电脑游戏。尽管如此，PyQt6 和其他库也可以用于创建简单的游戏。

制作一个电脑游戏是提高编程技能的好方法。

## 开发
因为没有游戏砖块的图片，所以这里使用 PyQt6 编程工具包中的绘图 API 来绘制砖块。每一款电脑游戏的背后都有一个数学模型，俄罗斯方块也是如此。

一些思路:

- 使用 `QtCore.QBasicTimer` 创建游戏循环
- 画出砖块
- 砖块整体旋转或移动（不是分开操作）
- 数学意义上，游戏面板是个简单的数字列表

代码包含四个类：`Tetris`，`Board`，`Tetrominoe` 和 `Shape`。`Tetris` 类设置了游戏。`Board` 是编写游戏逻辑的地方。`Tetrominoe` 类包含所有俄罗斯方块的名称，`Shape` 类包含俄罗斯方块的代码。

``` python
# files: tetris.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This is a Tetris game clone.

Author: Jan Bodnar
Website: zetcode.com
"""

import random
import sys

from PyQt6.QtCore import Qt, QBasicTimer, pyqtSignal
from PyQt6.QtGui import QPainter, QColor
from PyQt6.QtWidgets import QMainWindow, QFrame, QApplication


class Tetris(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):
        """initiates application UI"""

        self.tboard = Board(self)
        self.setCentralWidget(self.tboard)

        self.statusbar = self.statusBar()
        self.tboard.msg2Statusbar[str].connect(self.statusbar.showMessage)

        self.tboard.start()

        self.resize(180, 380)
        self.center()
        self.setWindowTitle('Tetris')
        self.show()


    def center(self):
        """centers the window on the screen"""

        qr = self.frameGeometry()
        cp = self.screen().availableGeometry().center()

        qr.moveCenter(cp)
        self.move(qr.topLeft())


class Board(QFrame):

    msg2Statusbar = pyqtSignal(str)

    BoardWidth = 10
    BoardHeight = 22
    Speed = 300


    def __init__(self, parent):
        super().__init__(parent)

        self.initBoard()


    def initBoard(self):
        """initiates board"""

        self.timer = QBasicTimer()
        self.isWaitingAfterLine = False

        self.curX = 0
        self.curY = 0
        self.numLinesRemoved = 0
        self.board = []

        self.setFocusPolicy(Qt.FocusPolicy.StrongFocus)
        self.isStarted = False
        self.isPaused = False
        self.clearBoard()


    def shapeAt(self, x, y):
        """determines shape at the board position"""

        return self.board[(y * Board.BoardWidth) + x]


    def setShapeAt(self, x, y, shape):
        """sets a shape at the board"""

        self.board[(y * Board.BoardWidth) + x] = shape


    def squareWidth(self):
        """returns the width of one square"""

        return self.contentsRect().width() // Board.BoardWidth


    def squareHeight(self):
        """returns the height of one square"""

        return self.contentsRect().height() // Board.BoardHeight


    def start(self):
        """starts game"""

        if self.isPaused:
            return

        self.isStarted = True
        self.isWaitingAfterLine = False
        self.numLinesRemoved = 0
        self.clearBoard()

        self.msg2Statusbar.emit(str(self.numLinesRemoved))

        self.newPiece()
        self.timer.start(Board.Speed, self)


    def pause(self):
        """pauses game"""

        if not self.isStarted:
            return

        self.isPaused = not self.isPaused

        if self.isPaused:
            self.timer.stop()
            self.msg2Statusbar.emit("paused")

        else:
            self.timer.start(Board.Speed, self)
            self.msg2Statusbar.emit(str(self.numLinesRemoved))

        self.update()


    def paintEvent(self, event):
        """paints all shapes of the game"""

        painter = QPainter(self)
        rect = self.contentsRect()

        boardTop = rect.bottom() - Board.BoardHeight * self.squareHeight()

        for i in range(Board.BoardHeight):
            for j in range(Board.BoardWidth):
                shape = self.shapeAt(j, Board.BoardHeight - i - 1)

                if shape != Tetrominoe.NoShape:
                    self.drawSquare(painter,
                                    rect.left() + j * self.squareWidth(),
                                    boardTop + i * self.squareHeight(), shape)

        if self.curPiece.shape() != Tetrominoe.NoShape:

            for i in range(4):
                x = self.curX + self.curPiece.x(i)
                y = self.curY - self.curPiece.y(i)
                self.drawSquare(painter, rect.left() + x * self.squareWidth(),
                            boardTop + (Board.BoardHeight - y - 1) * self.squareHeight(),
                            self.curPiece.shape())


    def keyPressEvent(self, event):
        """processes key press events"""

        if not self.isStarted or self.curPiece.shape() == Tetrominoe.NoShape:
            super(Board, self).keyPressEvent(event)
            return

        key = event.key()

        if key == Qt.Key.Key_P:
            self.pause()
            return

        if self.isPaused:
            return

        elif key == Qt.Key.Key_Left.value:
            self.tryMove(self.curPiece, self.curX - 1, self.curY)

        elif key == Qt.Key.Key_Right.value:
            self.tryMove(self.curPiece, self.curX + 1, self.curY)

        elif key == Qt.Key.Key_Down.value:
            self.tryMove(self.curPiece.rotateRight(), self.curX, self.curY)

        elif key == Qt.Key.Key_Up.value:
            self.tryMove(self.curPiece.rotateLeft(), self.curX, self.curY)

        elif key == Qt.Key.Key_Space.value:
            self.dropDown()

        elif key == Qt.Key.Key_D.value:
            self.oneLineDown()

        else:
            super(Board, self).keyPressEvent(event)


    def timerEvent(self, event):
        """handles timer event"""

        if event.timerId() == self.timer.timerId():

            if self.isWaitingAfterLine:
                self.isWaitingAfterLine = False
                self.newPiece()
            else:
                self.oneLineDown()

        else:
            super(Board, self).timerEvent(event)


    def clearBoard(self):
        """clears shapes from the board"""

        for i in range(Board.BoardHeight * Board.BoardWidth):
            self.board.append(Tetrominoe.NoShape)


    def dropDown(self):
        """drops down a shape"""

        newY = self.curY

        while newY > 0:

            if not self.tryMove(self.curPiece, self.curX, newY - 1):
                break

            newY -= 1

        self.pieceDropped()


    def oneLineDown(self):
        """goes one line down with a shape"""

        if not self.tryMove(self.curPiece, self.curX, self.curY - 1):
            self.pieceDropped()


    def pieceDropped(self):
        """after dropping shape, remove full lines and create new shape"""

        for i in range(4):
            x = self.curX + self.curPiece.x(i)
            y = self.curY - self.curPiece.y(i)
            self.setShapeAt(x, y, self.curPiece.shape())

        self.removeFullLines()

        if not self.isWaitingAfterLine:
            self.newPiece()


    def removeFullLines(self):
        """removes all full lines from the board"""

        numFullLines = 0
        rowsToRemove = []

        for i in range(Board.BoardHeight):

            n = 0
            for j in range(Board.BoardWidth):
                if not self.shapeAt(j, i) == Tetrominoe.NoShape:
                    n = n + 1

            if n == 10:
                rowsToRemove.append(i)

        rowsToRemove.reverse()

        for m in rowsToRemove:

            for k in range(m, Board.BoardHeight):
                for l in range(Board.BoardWidth):
                    self.setShapeAt(l, k, self.shapeAt(l, k + 1))

        numFullLines = numFullLines + len(rowsToRemove)

        if numFullLines > 0:
            self.numLinesRemoved = self.numLinesRemoved + numFullLines
            self.msg2Statusbar.emit(str(self.numLinesRemoved))

            self.isWaitingAfterLine = True
            self.curPiece.setShape(Tetrominoe.NoShape)
            self.update()


    def newPiece(self):
        """creates a new shape"""

        self.curPiece = Shape()
        self.curPiece.setRandomShape()
        self.curX = Board.BoardWidth // 2 + 1
        self.curY = Board.BoardHeight - 1 + self.curPiece.minY()

        if not self.tryMove(self.curPiece, self.curX, self.curY):

            self.curPiece.setShape(Tetrominoe.NoShape)
            self.timer.stop()
            self.isStarted = False
            self.msg2Statusbar.emit("Game over")


    def tryMove(self, newPiece, newX, newY):
        """tries to move a shape"""

        for i in range(4):

            x = newX + newPiece.x(i)
            y = newY - newPiece.y(i)

            if x < 0 or x >= Board.BoardWidth or y < 0 or y >= Board.BoardHeight:
                return False

            if self.shapeAt(x, y) != Tetrominoe.NoShape:
                return False

        self.curPiece = newPiece
        self.curX = newX
        self.curY = newY
        self.update()

        return True


    def drawSquare(self, painter, x, y, shape):
        """draws a square of a shape"""

        colorTable = [0x000000, 0xCC6666, 0x66CC66, 0x6666CC,
                      0xCCCC66, 0xCC66CC, 0x66CCCC, 0xDAAA00]

        color = QColor(colorTable[shape])
        painter.fillRect(x + 1, y + 1, self.squareWidth() - 2,
                         self.squareHeight() - 2, color)

        painter.setPen(color.lighter())
        painter.drawLine(x, y + self.squareHeight() - 1, x, y)
        painter.drawLine(x, y, x + self.squareWidth() - 1, y)

        painter.setPen(color.darker())
        painter.drawLine(x + 1, y + self.squareHeight() - 1,
                         x + self.squareWidth() - 1, y + self.squareHeight() - 1)
        painter.drawLine(x + self.squareWidth() - 1,
                         y + self.squareHeight() - 1, x + self.squareWidth() - 1, y + 1)


class Tetrominoe:

    NoShape = 0
    ZShape = 1
    SShape = 2
    LineShape = 3
    TShape = 4
    SquareShape = 5
    LShape = 6
    MirroredLShape = 7


class Shape:

    coordsTable = (
        ((0, 0), (0, 0), (0, 0), (0, 0)),
        ((0, -1), (0, 0), (-1, 0), (-1, 1)),
        ((0, -1), (0, 0), (1, 0), (1, 1)),
        ((0, -1), (0, 0), (0, 1), (0, 2)),
        ((-1, 0), (0, 0), (1, 0), (0, 1)),
        ((0, 0), (1, 0), (0, 1), (1, 1)),
        ((-1, -1), (0, -1), (0, 0), (0, 1)),
        ((1, -1), (0, -1), (0, 0), (0, 1))
    )

    def __init__(self):

        self.coords = [[0, 0] for i in range(4)]
        self.pieceShape = Tetrominoe.NoShape

        self.setShape(Tetrominoe.NoShape)


    def shape(self):
        """returns shape"""

        return self.pieceShape


    def setShape(self, shape):
        """sets a shape"""

        table = Shape.coordsTable[shape]

        for i in range(4):
            for j in range(2):
                self.coords[i][j] = table[i][j]

        self.pieceShape = shape


    def setRandomShape(self):
        """chooses a random shape"""

        self.setShape(random.randint(1, 7))


    def x(self, index):
        """returns x coordinate"""

        return self.coords[index][0]


    def y(self, index):
        """returns y coordinate"""

        return self.coords[index][1]


    def setX(self, index, x):
        """sets x coordinate"""

        self.coords[index][0] = x


    def setY(self, index, y):
        """sets y coordinate"""

        self.coords[index][1] = y
        

    def minX(self):
        """returns min x value"""

        m = self.coords[0][0]
        for i in range(4):
            m = min(m, self.coords[i][0])

        return m


    def maxX(self):
        """returns max x value"""

        m = self.coords[0][0]
        for i in range(4):
            m = max(m, self.coords[i][0])

        return m


    def minY(self):
        """returns min y value"""

        m = self.coords[0][1]
        for i in range(4):
            m = min(m, self.coords[i][1])

        return m


    def maxY(self):
        """returns max y value"""

        m = self.coords[0][1]
        for i in range(4):
            m = max(m, self.coords[i][1])

        return m


    def rotateLeft(self):
        """rotates shape to the left"""

        if self.pieceShape == Tetrominoe.SquareShape:
            return self

        result = Shape()
        result.pieceShape = self.pieceShape

        for i in range(4):
            result.setX(i, self.y(i))
            result.setY(i, -self.x(i))

        return result


    def rotateRight(self):
        """rotates shape to the right"""

        if self.pieceShape == Tetrominoe.SquareShape:
            return self

        result = Shape()
        result.pieceShape = self.pieceShape

        for i in range(4):
            result.setX(i, -self.y(i))
            result.setY(i, self.x(i))

        return result


def main():

    app = QApplication([])
    tetris = Tetris()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

游戏被简化了一点，这样更容易理解。游戏启动后立即开始。我们可以按 `P` 键暂停游戏。按空格键，方块会立即掉落到底部。游戏以固定的速度运行，没有任加速度。分数是消除的行数。

```python
self.tboard = Board(self)
self.setCentralWidget(self.tboard)
```
实例化 Board 类，并将其设置为应用程序的中心部件。

```python
self.statusbar = self.statusBar()
self.tboard.msg2Statusbar[str].connect(self.statusbar.showMessage)
```
这里创建一个状态栏，用于显示消息。我们将显示三种可能的消息:已经删除的行数，暂停消息，或游戏结束消息。`msg2Statusbar` 是在 Board 类中实现的自定义信号。`showMessage` 是一个内置方法，用于在状态栏上显示消息。

``` python
self.tboard.start()
```
初始化游戏。

```python
class Board(QFrame):

    msg2Statusbar = pyqtSignal(str)
...
```
`pyqtSignal` 创建了一个自定义信号。想在状态栏上展示信息或者分数，触发 `msg2Statusbar` 信号即可。

``` python
BoardWidth = 10
BoardHeight = 22
Speed = 300
```
这些是 `Board` 的参数。`BoardWidth` 和 `BoardHeight` 定义画板的宽高。 `Speed` 是游戏的速度，每300毫秒游戏循环进行一次。

``` python
...
self.curX = 0
self.curY = 0
self.numLinesRemoved = 0
self.board = []
...
```
在 `initBoard` 方法中，我们初始化一些变量。`self.board` 是一个从0到7的数字列表，代表了砖块的各种形状和位置信息。

``` python
def shapeAt(self, x, y):
    """determines shape at the board position"""

    return self.board[(y * Board.BoardWidth) + x]
```
`shapeAt` 方法决定了形状的位置。
```python
def squareWidth(self):
    """returns the width of one square"""

    return self.contentsRect().width() // Board.BoardWidth
```
画板可以动态调整大小。`squareWidth` 计算并返回单个形状的像素宽度。`Board.BoardWidth` 是画板的大小。

```python
def pause(self):
    """pauses game"""

    if not self.isStarted:
        return

    self.isPaused = not self.isPaused

    if self.isPaused:
        self.timer.stop()
        self.msg2Statusbar.emit("paused")

    else:
        self.timer.start(Board.Speed, self)
        self.msg2Statusbar.emit(str(self.numLinesRemoved))

    self.update()
```
`pause` 方法暂停游戏，停止计时并在状态栏上显示一个信息。

```python
def paintEvent(self, event):
    """paints all shapes of the game"""

    painter = QPainter(self)
    rect = self.contentsRect()
...
```
`paintEvent` 方法内绘制游戏。`QPainter` 是 PyQt6 里执行底层绘制的方法。

```python
for i in range(Board.BoardHeight):
    for j in range(Board.BoardWidth):
        shape = self.shapeAt(j, Board.BoardHeight - i - 1)

        if shape != Tetrominoe.NoShape:
            self.drawSquare(painter,
                rect.left() + j * self.squareWidth(),
                boardTop + i * self.squareHeight(), shape)
```
游戏绘制分为两个步骤。在第一步中，我们画出所有的形状，或者已经落在画板底部的形状。所有的方格都记录在 `self.board` 的变量列表。可以使用 `shapeAt` 方法访问该变量。

```python
if self.curPiece.shape() != Tetrominoe.NoShape:

    for i in range(4):

        x = self.curX + self.curPiece.x(i)
        y = self.curY - self.curPiece.y(i)
        self.drawSquare(painter, rect.left() + x * self.squareWidth(),
            boardTop + (Board.BoardHeight - y - 1) * self.squareHeight(),
            self.curPiece.shape())
```
第二步画出正在下落的砖块。

```python
elif key == Qt.Key.Key_Right.value:
    self.tryMove(self.curPiece, self.curX + 1, self.curY)
```
在 `keyPressEvent` 方法中，我们检查按下的键。如果按下右箭头键，将尝试部件向右移动。说“尝试”是因为它可能无法移动。

```python
elif key == Qt.Key.Key_Up.value:
    self.tryMove(self.curPiece.rotateLeft(), self.curX, self.curY)
```
`Up` 键向左旋转砖块。

```python
elif key == Qt.Key.Key_Space.value:
    self.dropDown()
```
`Space` 键会让砖块直接落到底部。

```python
elif key == Qt.Key.Key_D.value:
    self.oneLineDown()
```
按下D键，则会让砖块加速下落一会。

```python
def timerEvent(self, event):
    """handles timer event"""

    if event.timerId() == self.timer.timerId():

        if self.isWaitingAfterLine:
            self.isWaitingAfterLine = False
            self.newPiece()
        else:
            self.oneLineDown()

    else:
        super(Board, self).timerEvent(event)
```
在计时器事件中，要么在前一个砖块到底部之后创建一个新的，要么将一个砖块向下移动一行。

```python
def clearBoard(self):
    """clears shapes from the board"""

    for i in range(Board.BoardHeight * Board.BoardWidth):
        self.board.append(Tetrominoe.NoShape)
```
`clearBoard` 方法用设置 `Tetrominoe.NoShape` 的方式做到清空画板上的全部砖块。

```python
def removeFullLines(self):
    """removes all full lines from the board"""

    numFullLines = 0
    rowsToRemove = []

    for i in range(Board.BoardHeight):

        n = 0
        for j in range(Board.BoardWidth):
            if not self.shapeAt(j, i) == Tetrominoe.NoShape:
                n = n + 1

        if n == 10:
            rowsToRemove.append(i)

    rowsToRemove.reverse()


    for m in rowsToRemove:

        for k in range(m, Board.BoardHeight):
            for l in range(Board.BoardWidth):
                    self.setShapeAt(l, k, self.shapeAt(l, k + 1))

    numFullLines = numFullLines + len(rowsToRemove)
 ...
```
如果砖块落在底部，就调用 `removeFullLines` 方法，找出所有完整的一行并删除。将所有行移动到当前整行的位置上，做到删除的效果。注意，我们颠倒了要删除的行的顺序，不然会出现BUG，在我们的例子中，我们使用了 `naive gravity`，这意味着不是整行的砖块可能漂浮在空白的间隙之上。

```python
def newPiece(self):
    """creates a new shape"""

    self.curPiece = Shape()
    self.curPiece.setRandomShape()
    self.curX = Board.BoardWidth // 2 + 1
    self.curY = Board.BoardHeight - 1 + self.curPiece.minY()

    if not self.tryMove(self.curPiece, self.curX, self.curY):

        self.curPiece.setShape(Tetrominoe.NoShape)
        self.timer.stop()
        self.isStarted = False
        self.msg2Statusbar.emit("Game over")
```
`newPiece` 方法创建随机的砖块，如果创建的砖块不能到达初始位置，游戏结束。

``` python
def tryMove(self, newPiece, newX, newY):
    """tries to move a shape"""

    for i in range(4):

        x = newX + newPiece.x(i)
        y = newY - newPiece.y(i)

        if x < 0 or x >= Board.BoardWidth or y < 0 or y >= Board.BoardHeight:
            return False

        if self.shapeAt(x, y) != Tetrominoe.NoShape:
            return False

    self.curPiece = newPiece
    self.curX = newX
    self.curY = newY
    self.update()

    return True
```
`tryMove` 方法尝试移动一个砖块，如果砖块在画板或者另外一个砖块的边缘，返回 `False`。否则就执行移动。

``` python
class Tetrominoe:

    NoShape = 0
    ZShape = 1
    SShape = 2
    LineShape = 3
    TShape = 4
    SquareShape = 5
    LShape = 6
    MirroredLShape = 7
```
`Tetrominoe` 类包含了所有形状的名称，这里也有一个叫 `NoShape` 的空形状。

`Shape` 类保存了砖块的信息。

``` python
class Shape(object):

    coordsTable = (
        ((0, 0),     (0, 0),     (0, 0),     (0, 0)),
        ((0, -1),    (0, 0),     (-1, 0),    (-1, 1)),
        ...
    )
...
```
`coordsTable` 元组里包括了所有砖块的组合坐标，可以从这个模板里拼出需要的砖块。

```python
self.coords = [[0,0] for i in range(4)]
```
初始化时，创建一个空的坐标列表，用来存储砖块的坐标。

![Coordinates](./images/coordinates.png)

图示: 坐标

上面的图能帮助我们更好的理解坐标值的意义。比如，`(0, -1), (0, 0), (-1, 0), (-1, -1)
` 代表了Z字形的砖块，图示里就是这个形状。

``` python
def rotateLeft(self):
    """rotates shape to the left"""

    if self.pieceShape == Tetrominoe.SquareShape:
        return self

    result = Shape()
    result.pieceShape = self.pieceShape

    for i in range(4):

        result.setX(i, self.y(i))
        result.setY(i, -self.x(i))

    return result
```
`rotateLeft` 方法向左旋转砖块。方形没必要旋转，所以就直接返回当前的对象。当砖块发生旋转时，会产生一个新的对象代表这个旋转后的砖块。

![Tetris](./images/tetris.png)

图示: 俄罗斯方块