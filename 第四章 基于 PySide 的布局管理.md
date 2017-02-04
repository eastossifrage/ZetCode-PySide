# 第四章 基于 PySide 的布局管理

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。
本章主要介绍 GUI 编程中的重要事情——布局管理，这是一门如何将窗体小部件按照作者的意愿摆放在中心窗体上的学问，可以使用绝对定位或布局类两种方式来满足要求。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

## 绝对定位

当你使用绝对定位时，你必须了解几件事情。

- 如果我们调整窗体的大小，那么窗体中的小部件的大小和位置不会改变。
- 相同的应用程序在不同的平台上是有差异的。
- 更改应用程序中的字体可能会破坏布局。
- 不要轻易改变设计好的布局，重做的过程是乏味和耗时的。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows three labels on a window
using absolute positioning. 

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):
        
        label1 = QtGui.QLabel('Zetcode', self)
        label1.move(15, 10)

        label2 = QtGui.QLabel('tutorials', self)
        label2.move(35, 40)
        
        label3 = QtGui.QLabel('for programmers', self)
        label3.move(55, 70)        
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Absolute')    
        self.show()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

只需要调用 `move()` 方法来定位小部件。示例中，这些小部件是标签，通过 x 和 y 的坐标来定位。坐标系的开始位于左上角，x 值从左到右增长。y 值从上到下增长。

![absolute positioning](http://zetcode.com/img/gui/pyside/absolute.png)

## 框布局

使用布局类的布局管理方式更加灵活和实用。 这是在窗体上放置小部件的首选方式。 基本布局类是 QtGui.QHBoxLayout 和 QtGui.QVBoxLayout ，可水平和垂直排列小部件。

想象一下，在主窗体的右下角放置两个按钮。 要创建这样的布局，可以使用一个水平和一个垂直框。 同时，为了创建必要的空间，需添加一个拉伸系数。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we position two push
buttons in the bottom-right corner 
of the window. 

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):
        
        okButton = QtGui.QPushButton("OK")
        cancelButton = QtGui.QPushButton("Cancel")

        hbox = QtGui.QHBoxLayout()
        hbox.addStretch(1)
        hbox.addWidget(okButton)
        hbox.addWidget(cancelButton)

        vbox = QtGui.QVBoxLayout()
        vbox.addStretch(1)
        vbox.addLayout(hbox)
        
        self.setLayout(vbox)    
        
        self.setGeometry(300, 300, 300, 150)
        self.setWindowTitle('Buttons')    
        self.show()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

示例中将两个按钮放置在主窗体的右下角。即使我们调整应用程序窗体的大小，它们也保持位置不变。此处，使用到 QtGui.QHBoxLayout 和 QtGui.QVBoxLayout 两个布局类。

```python
okButton = QtGui.QPushButton("OK")
cancelButton = QtGui.QPushButton("Cancel")
```

创建两个按钮。

```python
hbox = QtGui.QHBoxLayout()
hbox.addStretch(1)
hbox.addWidget(okButton)
hbox.addWidget(cancelButton)
```

第一行创建一个水平框布局。第二行添加拉神系数。第三、四行添加两个按钮。此处在两个按钮之前增加了一个可拉神的空间，这将推动它们在窗体的右边。

```python
vbox = QtGui.QVBoxLayout()
vbox.addStretch(1)
vbox.addLayout(hbox)
```

为了创建必要的布局，需要将水平布局放到垂直布局中。垂直框中的拉伸系数将把带有按钮的水平框推到窗口的底部。

```python
self.setLayout(vbox)
```

最后，我们设置窗口的基本布局——垂直框。

![button](http://zetcode.com/img/gui/pyside/buttons.png)

## 网格布局

PySide 中最通用的布局类是网格布局。 此布局将空间划分为行和列。 创建网格布局需要使用QtGui.QGridLayout类。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we create a skeleton
of a calculator using a QGridLayout.

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):
        
        names = ['Cls', 'Bck', '', 'Close', '7', '8', '9', '/',
                '4', '5', '6', '*', '1', '2', '3', '-',
                '0', '.', '=', '+']

        grid = QtGui.QGridLayout()

        j = 0
        pos = [(0, 0), (0, 1), (0, 2), (0, 3),
                (1, 0), (1, 1), (1, 2), (1, 3),
                (2, 0), (2, 1), (2, 2), (2, 3),
                (3, 0), (3, 1), (3, 2), (3, 3 ),
                (4, 0), (4, 1), (4, 2), (4, 3)]

        for i in names:
            button = QtGui.QPushButton(i)
            if j == 2:
                grid.addWidget(QtGui.QLabel(''), 0, 2)
            else: 
                grid.addWidget(button, pos[j][0], pos[j][1])
            j = j + 1

        self.setLayout(grid)   
        
        self.move(300, 150)
        self.setWindowTitle('Calculator')    
        self.show()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例中，创建一个按钮网格。同时为了填补一个空白，还添加了一个 QtGui.QLabel 小部件。

```python
grid = QtGui.QGridLayout()
```

此处创建一个网格布局。

```python
if j == 2:
    grid.addWidget(QtGui.QLabel(''), 0, 2)
else: grid.addWidget(button, pos[j][0], pos[j][1])
```

将小部件添加到网格，需调用 `addWidget()` 方法。该方法的参数是小部件对象，网格的行号和列号。

![calculator](http://zetcode.com/img/gui/pyside/calculator.png)

## 评论框

小部件可以占用网格中的多个列或行。在下一个例子中，我们将说明这一点。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we create a bit
more complicated window layout using
the QGridLayout manager. 

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):
        
        title = QtGui.QLabel('Title')
        author = QtGui.QLabel('Author')
        review = QtGui.QLabel('Review')

        titleEdit = QtGui.QLineEdit()
        authorEdit = QtGui.QLineEdit()
        reviewEdit = QtGui.QTextEdit()

        grid = QtGui.QGridLayout()
        grid.setSpacing(10)

        grid.addWidget(title, 1, 0)
        grid.addWidget(titleEdit, 1, 1)

        grid.addWidget(author, 2, 0)
        grid.addWidget(authorEdit, 2, 1)

        grid.addWidget(review, 3, 0)
        grid.addWidget(reviewEdit, 3, 1, 5, 1)
        
        self.setLayout(grid) 
        
        self.setGeometry(300, 300, 350, 300)
        self.setWindowTitle('Review')    
        self.show()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

此处创建一个窗体，其中有三个标签，两个行编辑和一个文本编辑小部件。布局是用 QtGui.QGridLayout 完成的。

```python
grid = QtGui.QGridLayout()
grid.setSpacing(10)
```

创建一个网格布局，并在小部件之间设置间距。

```python
grid.addWidget(reviewEdit, 3, 1, 5, 1)
```

将一个小部件添加到网格，可以提供小部件的行跨度和列跨度。 在本示例中， reviewEdit 小部件将占用 5 行。

![review](http://zetcode.com/img/gui/pyside/review.png)

这一部分教程专门用于介绍布局管理。