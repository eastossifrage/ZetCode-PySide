# 第十二章 基于 PySide 制作俄罗斯方块游戏

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

编写电脑游戏是一项很有挑战性的工作。作为程序员，迟早会想要创建一款电脑游戏。事实上，许多人对编程感兴趣，是因为他们玩游戏，并且想要创建自己的游戏。**创建一个电脑有助于提高你的编程能力。**

## 俄罗斯方块

俄罗斯方块游戏是有史以来最流行的电脑游戏之一。原始游戏是由俄罗斯程序员 Alexey Pajitnov 在 1985 年设计和创造的，从那时起，俄罗斯方块游戏几乎存在于每个计算机平台上。

俄罗斯方块被称为一个下降块益智游戏。在这个游戏中，有七个不同的形状，称为 tetrominoes。S 形，Z 形，T 形，L 形，线形，镜面 L 形和正方形。这些形状中的每一个由四个正方形小块形成，最终会落下到屏幕最下方。俄罗斯方块游戏的目的是移动和旋转形状，使它们尽可能降落在适合的位置，如果设法形成一行，则该行被销毁，用户得分，用户可以一直玩下去，直到方块累计得超出屏幕。

![Tetrominoes](http://zetcode.com/img/gui/pyside/tetrominoes.png)

创建电脑游戏可以使用专用的框架结构及工具包。但是 PySide 是一个用于创建应用程序的工具包，也可以用来创建简单的游戏。

## 发展

俄罗斯方块游戏的图像，是使用PySide编程工具包中提供的绘图API绘制的。每个计算机游戏都有一个数学模型，俄罗斯方块也不例外。

设计游戏的思路：

- 使用 `QtCore.QBasicTimer()` 创建一个游戏循环。
- 绘制俄罗斯方块。
- 一格一格的移动方块的位置（不是逐个像素）。
- 数学上，这是一个简单的数字列表。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial

This is a simple Tetris clone
in PySide.

author: Jan Bodnar
website: zetcode.com
last edited: August 2011
"""

import sys, random
from PySide import QtCore, QtGui

class Communicate(QtCore.QObject):
    
    msgToSB = QtCore.Signal(str)

class Tetris(QtGui.QMainWindow):
    
    def __init__(self):
        super(Tetris, self).__init__()

        self.setGeometry(300, 300, 180, 380)
        self.setWindowTitle('Tetris')
        self.Tetrisboard = Board(self)

        self.setCentralWidget(self.Tetrisboard)

        self.statusbar = self.statusBar()
        self.Tetrisboard.c.msgToSB[str].connect(self.statusbar.showMessage)
            
        self.Tetrisboard.start()
        self.center()

    def center(self):
        
        screen = QtGui.QDesktopWidget().screenGeometry()
        size =  self.geometry()
        self.move((screen.width()-size.width())/2, 
            (screen.height()-size.height())/2)


class Board(QtGui.QFrame):
    
    BoardWidth = 10
    BoardHeight = 22
    Speed = 300

    def __init__(self, parent):
        super(Board, self).__init__()

        self.timer = QtCore.QBasicTimer()
        self.isWaitingAfterLine = False
        self.curPiece = Shape()
        self.nextPiece = Shape()
        self.curX = 0
        self.curY = 0
        self.numLinesRemoved = 0
        self.board = []

        self.setFocusPolicy(QtCore.Qt.StrongFocus)
        self.isStarted = False
        self.isPaused = False
        self.clearBoard()
        
        self.c = Communicate()

        self.nextPiece.setRandomShape()

    def shapeAt(self, x, y):
        return self.board[(y * Board.BoardWidth) + x]

    def setShapeAt(self, x, y, shape):
        self.board[(y * Board.BoardWidth) + x] = shape

    def squareWidth(self):
        return self.contentsRect().width() / Board.BoardWidth

    def squareHeight(self):
        return self.contentsRect().height() / Board.BoardHeight

    def start(self):
        if self.isPaused:
            return

        self.isStarted = True
        self.isWaitingAfterLine = False
        self.numLinesRemoved = 0
        self.clearBoard()

        self.c.msgToSB.emit(str(self.numLinesRemoved))

        self.newPiece()
        self.timer.start(Board.Speed, self)

    def pause(self):
        
        if not self.isStarted:
            return

        self.isPaused = not self.isPaused
        
        if self.isPaused:
            self.timer.stop()
            self.c.msgToSB.emit("paused")
        else:
            self.timer.start(Board.Speed, self)
            self.c.msgToSB.emit(str(self.numLinesRemoved))

        self.update()

    def paintEvent(self, event):
        
        painter = QtGui.QPainter(self)
        rect = self.contentsRect()

        boardTop = rect.bottom() - Board.BoardHeight * self.squareHeight()

        for i in range(Board.BoardHeight):
            for j in range(Board.BoardWidth):
                shape = self.shapeAt(j, Board.BoardHeight - i - 1)
                if shape != Tetrominoes.NoShape:
                    self.drawSquare(painter,
                        rect.left() + j * self.squareWidth(),
                        boardTop + i * self.squareHeight(), shape)

        if self.curPiece.shape() != Tetrominoes.NoShape:
            for i in range(4):
                x = self.curX + self.curPiece.x(i)
                y = self.curY - self.curPiece.y(i)
                self.drawSquare(painter, rect.left() + x * self.squareWidth(),
                    boardTop + (Board.BoardHeight - y - 1) * self.squareHeight(),
                    self.curPiece.shape())

    def keyPressEvent(self, event):
        
        if not self.isStarted or self.curPiece.shape() == Tetrominoes.NoShape:
            QtGui.QWidget.keyPressEvent(self, event)
            return

        key = event.key()
        
        if key == QtCore.Qt.Key_P:
            self.pause()
            return
        if self.isPaused:
            return
        elif key == QtCore.Qt.Key_Left:
            self.tryMove(self.curPiece, self.curX - 1, self.curY)
        elif key == QtCore.Qt.Key_Right:
            self.tryMove(self.curPiece, self.curX + 1, self.curY)
        elif key == QtCore.Qt.Key_Down:
            self.tryMove(self.curPiece.rotatedRight(), self.curX, self.curY)
        elif key == QtCore.Qt.Key_Up:
            self.tryMove(self.curPiece.rotatedLeft(), self.curX, self.curY)
        elif key == QtCore.Qt.Key_Space:
            self.dropDown()
        elif key == QtCore.Qt.Key_D:
            self.oneLineDown()
        else:
            QtGui.QWidget.keyPressEvent(self, event)

    def timerEvent(self, event):
        
        if event.timerId() == self.timer.timerId():
            if self.isWaitingAfterLine:
                self.isWaitingAfterLine = False
                self.newPiece()
            else:
                self.oneLineDown()
        else:
            QtGui.QFrame.timerEvent(self, event)

    def clearBoard(self):
        
        for i in range(Board.BoardHeight * Board.BoardWidth):
            self.board.append(Tetrominoes.NoShape)

    def dropDown(self):
        
        newY = self.curY
        while newY > 0:
            if not self.tryMove(self.curPiece, self.curX, newY - 1):
                break
            newY -= 1

        self.pieceDropped()

    def oneLineDown(self):
        
        if not self.tryMove(self.curPiece, self.curX, self.curY - 1):
            self.pieceDropped()

    def pieceDropped(self):
        
        for i in range(4):
            x = self.curX + self.curPiece.x(i)
            y = self.curY - self.curPiece.y(i)
            self.setShapeAt(x, y, self.curPiece.shape())

        self.removeFullLines()

        if not self.isWaitingAfterLine:
            self.newPiece()

    def removeFullLines(self):
        numFullLines = 0

        rowsToRemove = []

        for i in range(Board.BoardHeight):
            n = 0
            for j in range(Board.BoardWidth):
                if not self.shapeAt(j, i) == Tetrominoes.NoShape:
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
            print self.numLinesRemoved
            self.c.msgToSB.emit(str(self.numLinesRemoved))
            self.isWaitingAfterLine = True
            self.curPiece.setShape(Tetrominoes.NoShape)
            self.update()

    def newPiece(self):
        
        self.curPiece = self.nextPiece
        self.nextPiece.setRandomShape()
        self.curX = Board.BoardWidth / 2 + 1
        self.curY = Board.BoardHeight - 1 + self.curPiece.minY()

        if not self.tryMove(self.curPiece, self.curX, self.curY):
            self.curPiece.setShape(Tetrominoes.NoShape)
            self.timer.stop()
            self.isStarted = False
            self.c.msgToSB.emit("Game over")

    def tryMove(self, newPiece, newX, newY):
        
        for i in range(4):
            x = newX + newPiece.x(i)
            y = newY - newPiece.y(i)
            if x < 0 or x >= Board.BoardWidth or y < 0 or y >= Board.BoardHeight:
                return False
            if self.shapeAt(x, y) != Tetrominoes.NoShape:
                return False

        self.curPiece = newPiece
        self.curX = newX
        self.curY = newY
        self.update()
        return True

    def drawSquare(self, painter, x, y, shape):
        
        colorTable = [0x000000, 0xCC6666, 0x66CC66, 0x6666CC,
                      0xCCCC66, 0xCC66CC, 0x66CCCC, 0xDAAA00]

        color = QtGui.QColor(colorTable[shape])
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


class Tetrominoes(object):
    
    NoShape = 0
    ZShape = 1
    SShape = 2
    LineShape = 3
    TShape = 4
    SquareShape = 5
    LShape = 6
    MirroredLShape = 7


class Shape(object):
    
    coordsTable = (
        ((0, 0),     (0, 0),     (0, 0),     (0, 0)),
        ((0, -1),    (0, 0),     (-1, 0),    (-1, 1)),
        ((0, -1),    (0, 0),     (1, 0),     (1, 1)),
        ((0, -1),    (0, 0),     (0, 1),     (0, 2)),
        ((-1, 0),    (0, 0),     (1, 0),     (0, 1)),
        ((0, 0),     (1, 0),     (0, 1),     (1, 1)),
        ((-1, -1),   (0, -1),    (0, 0),     (0, 1)),
        ((1, -1),    (0, -1),    (0, 0),     (0, 1))
    )

    def __init__(self):
        
        self.coords = [[0,0] for i in range(4)]
        self.pieceShape = Tetrominoes.NoShape

        self.setShape(Tetrominoes.NoShape)

    def shape(self):
        return self.pieceShape

    def setShape(self, shape):
        
        table = Shape.coordsTable[shape]
        for i in range(4):
            for j in range(2):
                self.coords[i][j] = table[i][j]

        self.pieceShape = shape

    def setRandomShape(self):
        self.setShape(random.randint(1, 7))

    def x(self, index):
        return self.coords[index][0]

    def y(self, index):
        return self.coords[index][1]

    def setX(self, index, x):
        self.coords[index][0] = x

    def setY(self, index, y):
        self.coords[index][1] = y

    def minX(self):
        
        m = self.coords[0][0]
        for i in range(4):
            m = min(m, self.coords[i][0])

        return m

    def maxX(self):
        
        m = self.coords[0][0]
        for i in range(4):
            m = max(m, self.coords[i][0])

        return m

    def minY(self):
        
        m = self.coords[0][1]
        for i in range(4):
            m = min(m, self.coords[i][1])

        return m

    def maxY(self):
        
        m = self.coords[0][1]
        for i in range(4):
            m = max(m, self.coords[i][1])

        return m

    def rotatedLeft(self):
        
        if self.pieceShape == Tetrominoes.SquareShape:
            return self

        result = Shape()
        result.pieceShape = self.pieceShape
        
        for i in range(4):
            result.setX(i, self.y(i))
            result.setY(i, -self.x(i))

        return result

    def rotatedRight(self):
        
        if self.pieceShape == Tetrominoes.SquareShape:
            return self

        result = Shape()
        result.pieceShape = self.pieceShape
        
        for i in range(4):
            result.setX(i, -self.y(i))
            result.setY(i, self.x(i))

        return result

def main():
    
    app = QtGui.QApplication(sys.argv)
    t = Tetris()
    t.show()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

作者已经简化了游戏，让它更加容易理解。程序启动后游戏立即开始。用户可以通过 p 键暂停游戏，空格键则会将俄罗斯方块立即下降到底部。游戏以恒定速度进行，不实现加速。分数是我们已删除的行数。

```python
self.statusbar = self.statusBar()
self.Tetrisboard.c.msgToSB[str].connect(self.statusbar.showMessage)	
```

创建一个状态栏，用于显示消息 —— 已删除的行数，暂停和游戏结束。

```python
...
self.curX = 0
self.curY = 0
self.numLinesRemoved = 0
self.board = []
...
```

在开始游戏之前，初始化一些重要的变量。self.board 变量是从 0 到 7 的数字的列表。它表示“板”上的当前方块的各种形状和所处位置。

```python
for j in range(Board.BoardWidth):
    shape = self.shapeAt(j, Board.BoardHeight - i - 1)
    if shape != Tetrominoes.NoShape:
        self.drawSquare(painter,
            rect.left() + j * self.squareWidth(),
            boardTop + i * self.squareHeight(), shape)
```

绘制游戏的画面分为两个步骤，第一步，绘制所有的形状，或保留已经掉到底部的形状。所有的方块形状，都会被记录在 self.board 列表里，使用 `shapeAt()` 方法来访问它。

```python
if self.curPiece.shape() != Tetrominoes.NoShape:
    for i in range(4):
        x = self.curX + self.curPiece.x(i)
        y = self.curY - self.curPiece.y(i)
        self.drawSquare(painter, rect.left() + x * self.squareWidth(),
            boardTop + (Board.BoardHeight - y - 1) * self.squareHeight(),
            self.curPiece.shape())
```

下一步是绘制方块降落的实际位置。

```python
elif key == QtCore.Qt.Key_Left:
    self.tryMove(self.curPiece, self.curX - 1, self.curY)
elif key == QtCore.Qt.Key_Right:
    self.tryMove(self.curPiece, self.curX + 1, self.curY)
```

在 keyPressEvent 中，将检查按下的键。如果用户按下右箭头键，该方块将试图向右移动。

```python
def tryMove(self, newPiece, newX, newY):
    for i in range(4):
        x = newX + newPiece.x(i)
        y = newY - newPiece.y(i)
        if x < 0 or x >= Board.BoardWidth or y < 0 or y >= Board.BoardHeight:
            return False
        if self.shapeAt(x, y) != Tetrominoes.NoShape:
            return False

    self.curPiece = newPiece
    self.curX = newX
    self.curY = newY
    self.update()
    return True
```

在 `tryMove()` 方法中，将尝试移动当前方块。如果方块已在最底层或下降到无法下降的位置，则返回假。否则，将一直下降到无法再下降的新位置。

```python
def timerEvent(self, event):
    if event.timerId() == self.timer.timerId():
        if self.isWaitingAfterLine:
            self.isWaitingAfterLine = False
            self.newPiece()
        else:
            self.oneLineDown()
    else:
        QtGui.QFrame.timerEvent(self, event)
```

在定时器事件中，当上一个方块移动到最底部，或根据规则删除一行后，要创建新的方块。

```python
def removeFullLines(self):
    numFullLines = 0

    rowsToRemove = []

    for i in range(Board.BoardHeight):
        n = 0
        for j in range(Board.BoardWidth):
            if not self.shapeAt(j, i) == Tetrominoes.NoShape:
                n = n + 1

        if n == 10:
            rowsToRemove.append(i)

    rowsToRemove.reverse()

    for m in rowsToRemove:
        for k in range(m, Board.BoardHeight):
            for l in range(Board.BoardWidth):
                self.setShapeAt(l, k, self.shapeAt(l, k + 1))
...
```

调用 `removeFullLines()` 方法删除组成实线的行。如果有两行或以上的实线行，要先删除上面的实线行，在代码实现中，反转了要删除的行的顺序。

```python
def newPiece(self):
    
    self.curPiece = self.nextPiece
    self.nextPiece.setRandomShape()
    self.curX = Board.BoardWidth / 2 + 1
    self.curY = Board.BoardHeight - 1 + self.curPiece.minY()

    if not self.tryMove(self.curPiece, self.curX, self.curY):
        self.curPiece.setShape(Tetrominoes.NoShape)
        self.timer.stop()
        self.isStarted = False
        self.c.msgToSB.emit("Game over")
```

通过 `newPiece()` 方法随机创建一个新的俄罗斯方块，如果该方块无法进入其初始化位置，会触发“游戏结束”事件。

Shape类保存了有关俄罗斯方块的信息。

```python
self.coords = [[0,0] for i in range(4)]
```

当方块创建后，程序会新创建一个空的坐标列表。该列表保存了俄罗斯方块的坐标。例如，这些元组（0，-1），（0,0），（1,0），（1,1）表示旋转的S形。下图说明了形状。

![Coordinates](http://zetcode.com/img/gui/pyside/coordinates.png)

当绘制当前正在下降的方块时，需绘制出 self.curX，self.curY 位置坐标。然后根据坐标表，绘制出所有的四个小方块。

![Tetris](http://zetcode.com/img/gui/pyside/tetris.png)