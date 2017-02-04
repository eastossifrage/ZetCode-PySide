# 第三章 基于 PySide 的菜单和工具栏

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。
本章我们将讲解如何创建菜单和工具栏

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

## 主窗体

QtGui.QMainWindow 类提供了一个主应用程序窗口。这使得能够创建具有状态栏，工具栏和菜单的经典应用程序框架成为可能。

## 状态栏

状态栏是用于显示状态信息的窗体小部件。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This program creates a statusbar.

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui

class Example(QtGui.QMainWindow):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):               
        
        self.statusBar().showMessage('Ready')
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Statusbar')    
        self.show()
        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

通过 QtGui.QMainWindow 小部件的帮助可创建状态栏。

```python
self.statusBar().showMessage('Ready')
```

调用 tGui.QMainWindow 类的 `statusBar（）` 方法可获取状态栏。 该方法的第一次调用创建状态栏，后续调用返回状态栏对象，  `showMessage（）` 在状态栏上显示一条消息。

## 菜单栏

菜单栏是 GUI 应用程序的常见部分，菜单项是位于菜单栏中的各个指令。在脚本命令中，我们需要记住各种命令及其选项，在此，我们将原有的脚本命令通过一定逻辑关系演转化为图形界面执行。这种标准已经被广泛的接受，可进一步减少学习新的应用程序所花费的时间。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This program creates a menubar.

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui

class Example(QtGui.QMainWindow):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):               
        
        exitAction = QtGui.QAction(QtGui.QIcon('exit.png'), '&Exit', self)
        exitAction.setShortcut('Ctrl+Q')
        exitAction.setStatusTip('Exit application')
        exitAction.triggered.connect(self.close)

        self.statusBar()

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(exitAction)
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Menubar')    
        self.show()
        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在上面的例子中，我们创建了状态栏和一个菜单栏，其中包含一个菜单。 此菜单将包含一个操作，如果选择该菜单，将终止应用程序，可以使用 Ctrl + Q 快捷键访问该操作。


```python
exitAction = QtGui.QAction(QtGui.QIcon('exit.png'), '&Exit', self)
exitAction.setShortcut('Ctrl+Q')
exitAction.setStatusTip('Exit application')
```

QtGui.QAction 是使用菜单栏，工具栏或自定义快捷键所执行操作的抽象化概念。在上面的三行中，第一行是创建一个将要执行的操作，其带有一个特定的图标和一个“退出”标签，第二行是为此操作定义快捷方式，第三行是创建状态提示，当鼠标指针悬停在菜单项上时，在状态栏中显示提示信息。**第三行代码在 ubuntu 16.04 系统中不起作用。在 linuxmint 18 中显示正常。**

```python
exitAction.triggered.connect(self.close)
```

当我们选择菜单栏中“特定的动作”（此处特指 exitAction ）时，会发出一个触发信号。信号连接到 QtGui.QMainWindow 窗体小部件的 `close（）` 方法，最终会终止应用程序。

```python
menubar = self.menuBar()
```

创建一个菜单栏。

```python
fileMenu = menubar.addMenu('&File')
fileMenu.addAction(exitAction)
```

第一行是使用菜单栏对象的 `addMenu（）`方法创建一个文件菜单。 第二行是将先前创建的操作添加到文件菜单。

## 工具栏

菜单栏可以运行所有在应用程序中使用的命令。工具栏提供对最常用命令的快速访问的功能。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This program creates a toolbar.

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui

class Example(QtGui.QMainWindow):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):               
        
        exitAction = QtGui.QAction(QtGui.QIcon('exit24.png'), 'Exit', self)
        exitAction.setShortcut('Ctrl+Q')
        exitAction.triggered.connect(self.close)
        
        self.toolbar = self.addToolBar('Exit')
        self.toolbar.addAction(exitAction)
        
        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Toolbar')    
        self.show()
        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在上面的例子中，创建一个简单的工具栏，工具栏有一个操作—— 退出动作，关闭应用程序。

```python
exitAction = QtGui.QAction(QtGui.QIcon('exit24.png'), 'Exit', self)
exitAction.setShortcut('Ctrl+Q')
exitAction.triggered.connect(self.close)
```

类似于上面的菜单栏示例，我们创建一个操作对象, 其中有标签，图标和缩略图。 QtGui.QMainWindow 的 `close()` 方法连接到触发信号。

```python
self.toolbar = self.addToolBar('Exit')
self.toolbar.addAction(exitAction)
```

第一行是使用工具栏对象的 `addToolBar（）`方法创建一个工具栏插件。 第二行是将先前创建的操作添加到工具栏插件。

![toolbar](http://zetcode.com/img/gui/pyside/toolbar.png)

## 混合使用

在本节的最后一个例子中，我们将创建一个带有菜单栏，工具栏和状态栏的窗体小部件。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This program creates a skeleton of
a classic GUI application with a menubar,
toolbar, statusbar and a central widget. 

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui

class Example(QtGui.QMainWindow):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):               
        
        textEdit = QtGui.QTextEdit()
        self.setCentralWidget(textEdit)

        exitAction = QtGui.QAction('Exit', self)
        exitAction.setShortcut('Ctrl+Q')
        exitAction.setStatusTip('Exit application')
        exitAction.triggered.connect(self.close)

        self.statusBar()

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(exitAction)

        toolbar = self.addToolBar('Exit')
        toolbar.addAction(exitAction)
        
        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Main window')    
        self.show()
        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

此代码示例使用菜单栏、工具栏和状态栏创建经典 GUI 应用程序的基本骨架。

```python
textEdit = QtGui.QTextEdit()
self.setCentralWidget(textEdit)
```

第一行将创建一个文本编辑小部件。第二行将文本编辑小部件设置为 QtGui.QMainWindow 的中心窗口部件，并占据所有剩余的空间。

![QtGui.QMainWindow](http://zetcode.com/img/gui/pyside/mainwindow.png)


