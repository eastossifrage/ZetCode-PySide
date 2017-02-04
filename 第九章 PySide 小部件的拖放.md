# 第九章 PySide 小部件的拖放

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

在计算机图形用户界面中，拖放是指点击虚拟对象并将其拖动到不同位置或到另一虚拟对象上的动作。一般来说，它可以用于调用多种操作，或者在两个抽象对象之间创建各种类型的关联。 （维基百科）

拖放功能是图形用户界面最明显的特征之一。拖放操作使用户能够直观地完成复杂的事情。

通常，我们可以拖放两种东西——数据或一些图形对象。 如果我们将图像从一个应用程序拖动到另一个应用程序，我们拖放的是二进制数据。 如果我们在 Firefox 中拖动一个选项卡并将其移动到另一个地方，我们拖放的是一个图形组件。

## 简单的拖放

在第一个例子中，有一个 QtGui.QLineEdit 和一个 QtGui.QPushButton ，实现了从行编辑窗体小部件拖动纯文本并将其放在按钮窗体小部件上的功能。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This is a simple drag and
drop example.  

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui, QtCore

class Button(QtGui.QPushButton):
  
    def __init__(self, title, parent):
        super(Button, self).__init__(title, parent)
        
        self.setAcceptDrops(True)

    def dragEnterEvent(self, e):
      
        if e.mimeData().hasFormat('text/plain'):
            e.accept()
        else:
            e.ignore() 

    def dropEvent(self, e):
        self.setText(e.mimeData().text())
        

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):      
    
        qe = QtGui.QLineEdit('', self)
        qe.setDragEnabled(True)
        qe.move(30, 65)

        button = Button("Button", self)
        button.move(190, 65) 

        self.setGeometry(300, 300, 300, 150)
        self.setWindowTitle('Simple Drag & Drop')
        self.show()              
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

简单的拖放操作。

```python
class Button(QtGui.QPushButton):
  
    def __init__(self, title, parent):
        super(Button, self).__init__(title, parent)
```

为了能够将文本内容拖放到 QtGui.QPushButton 小部件上，必须得重新实现自定义的 Button 类，它将继承自 QtGui.QPushButton 类。

```python
self.setAcceptDrops(True)
```

为本窗体小部件启用 drop 事件。

```python
def dragEnterEvent(self, e):
  
    if e.mimeData().hasFormat('text/plain'):
        e.accept()
    else:
        e.ignore() 
```

该代码是重新实现 `dragEnterEvent()` 方法，通过对数据类型进行判断，决定是否接收。在本例中数据类型是纯文本。

```python
def dropEvent(self, e):
    self.setText(e.mimeData().text()) 
```

通过实现 `dropEvent()` 方法，定义对drop事件做出何种反映。本例是改变按钮小部件的文本。

```python
qe = QtGui.QLineEdit('', self)
qe.setDragEnabled(True)
```

QtGui.QLineEdit 小部件内置了对拖动操作的支持。此处需要做的就是调用 `setDragEnabled()`方法来激活该功能。

![Simple Drag & Drop](http://zetcode.com/img/gui/pyside/simpledd.png)

## 拖放按钮小部件

在小面的例子中，将演示如何拖放按钮小部件。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we drag and drop a
button.

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui, QtCore

class Button(QtGui.QPushButton):
  
    def __init__(self, title, parent):
        super(Button, self).__init__(title, parent)

    def mouseMoveEvent(self, e):

        if e.buttons() != QtCore.Qt.RightButton:
            return

        mimeData = QtCore.QMimeData()

        drag = QtGui.QDrag(self)
        drag.setMimeData(mimeData)
        drag.setHotSpot(e.pos() - self.rect().topLeft())

        dropAction = drag.start(QtCore.Qt.MoveAction)


    def mousePressEvent(self, e):
      
        QtGui.QPushButton.mousePressEvent(self, e)
        if e.button() == QtCore.Qt.LeftButton:
            print 'press'
        

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):      
    
        self.setAcceptDrops(True)

        self.btn = Button('Button', self)
        self.btn.move(100, 65)

        self.setGeometry(300, 300, 300, 150)
        self.setWindowTitle('Click or move')
        self.show()

    def dragEnterEvent(self, e):
      
        e.accept()

    def dropEvent(self, e):

        position = e.pos()
        self.btn.move(position)

        e.setDropAction(QtCore.Qt.MoveAction)
        e.accept()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在本例中，主窗体上有一个 QtGui.QPushButton 。当用户用鼠标左键点击按钮时，控制台将收到 “press” 信息。当右键点击并以东按钮，将对按钮小部件执行拖放操作。

```python
class Button(QtGui.QPushButton):
  
    def __init__(self, title, parent):
        super(Button, self).__init__(title, parent)
```

创建一个从 QtGui.QPushButton 派生的 Button 类。同时，在该类里还重新实现了 `mouseMoveEvent()` 和 `mousePressEvent()` 两个方法。`mouseMoveEvent()` 方法是为了确定开始拖放操作的位置。

```python
if event.buttons() != QtCore.Qt.RightButton:
    return
```

此处定义只能用鼠标右键执行拖放，鼠标左键仅保留点击按钮的功能。

```python
mimeData = QtCore.QMimeData()

drag = QtGui.QDrag(self)
drag.setMimeData(mimeData)
drag.setHotSpot(event.pos() - self.rect().topLeft())
```

创建 a QtGui.QDrag 对象。

```python
dropAction = drag.start(QtCore.Qt.MoveAction)
```

使用拖动对象的 `start()` 方法来执行拖放操作。

```python
def mousePressEvent(self, e):
  
    QtGui.QPushButton.mousePressEvent(self, e)
    if e.button() == QtCore.Qt.LeftButton:
        print 'press'
```

当用户点击鼠标左键的时候，在控制台显示 “press”。注意，父类也调用 `mousePressEvent()` 方法。 否则，将不会看到按钮被点击的动作。

```python
position = e.pos()

self.btn.move(position)
```

在 `dropEvent()` 方法中，定义了释放鼠标按钮，并完成放置操作之后，将要发生的事——找出当前的鼠标指针为值，并相应的以东按钮。

```python
e.setDropAction(QtCore.Qt.MoveAction)
e.accept()
```
指定放置操作的类型，在本例中，是一个移动动作。





