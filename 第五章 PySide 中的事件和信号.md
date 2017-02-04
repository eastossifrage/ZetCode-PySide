# 第五章 PySide 中的事件和信号

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。
本章教程，我们将探索应用程序中发生的事件和信号。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

## 事件

事件对于任何 GUI 程序来说都是很重要的一部分。事件是由用户或系统生成的。当调用应用程序的 `exec_（）` 方法时，应用程序进入主循环。主循环获取事件并将它们发送到相应的对象。

PySide 具有独特的信号和时隙机制。通过本章内容，你可以好好体会。

所有 GUI 应用程序都是事件驱动的，应用程序对在其生命期内生成的不同事件类型做出相应的反应。 事件*主要*用于程序和用户进行交互。 但是它们也可以通过其他方式生成。 例如 互联网连接，窗口管理器，定时器。 在事件模型中，有三个参与者：

- 事件源
- 事件对象
- 事件目标

对象的状态改变即事件源，将生成事件。事件对象（Event）则封装事件源中的状态更改。事件目标是要通知的对象。事件处理的第一步就是事件源对象将处理事件的任务委托给事件目标。

调用 `exec_()` 方法，应用程序将进入主循环。主循环获取事件并利用信号和时隙将事件发送至对象。当特定事件发生时，会促使信号发射，这中间需要调用一个时隙。

## 信号&时隙

这个示例将展示信号和时隙。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we connect a signal
of a QtGui.QSlider to a slot 
of a QtGui.QLCDNumber. 

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui, QtCore

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):
        
        lcd = QtGui.QLCDNumber(self)
        sld = QtGui.QSlider(QtCore.Qt.Horizontal, self)

        vbox = QtGui.QVBoxLayout()
        vbox.addWidget(lcd)
        vbox.addWidget(sld)

        self.setLayout(vbox)
        sld.valueChanged.connect(lcd.display)
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Signal & slot')
        self.show()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例中，展示了 QtGui.QLCDNumber 和 QtGui.QSlider 两个类。 可通过拖动滑动按钮来更改液晶数字。

```python
sld.valueChanged.connect(lcd.display)
```

此处，通过滑动按钮的 valueChanged 信号连接到 lcd 数字的显示时隙。

发送者 sld 是发送信号的对象，接收器 lcd 是接受信号的对象，时隙是对信号作出反映的方法。

![signal&slot](http://zetcode.com/img/gui/pyside/sigslot.png)

## 重新实现事件处理程序

事件通常通过重新实现事件处理程序来处理。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we reimplement an 
event handler. 

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui, QtCore

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):      
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Event handler')
        self.show()
        
    def keyPressEvent(self, e):
        
        if e.key() == QtCore.Qt.Key_Escape:
            self.close()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中，我们重新实现了 `keyPressEvent()` 的事件处理程序。

```python
def keyPressEvent(self, e):
    
    if e.key() == QtCore.Qt.Key_Escape:
        self.close()
```

当我们按下键盘上的 “Esc” 键的时候，程序会退出。

## 事件发送者

想知道哪个窗体小部件是信号的发送者是很方便的事。PySide 有一个 `sender()` 方法。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we determine the event sender
object.

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui, QtCore

class Example(QtGui.QMainWindow):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):      

        btn1 = QtGui.QPushButton("Button 1", self)
        btn1.move(30, 50)

        btn2 = QtGui.QPushButton("Button 2", self)
        btn2.move(150, 50)
      
        btn1.clicked.connect(self.buttonClicked)            
        btn2.clicked.connect(self.buttonClicked)
        
        self.statusBar()
        
        self.setGeometry(300, 300, 290, 150)
        self.setWindowTitle('Event sender')
        self.show()
        
    def buttonClicked(self):
      
        sender = self.sender()
        self.statusBar().showMessage(sender.text() + ' was pressed')
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中有两个按钮。 `buttonClicked()` 方法定义，被点击过的按钮会调用 `sender()` 方法。

```python
btn1.clicked.connect(self.buttonClicked)            
btn2.clicked.connect(self.buttonClicked)
```

所有的按钮都连接到同一个时隙。

```python
def buttonClicked(self):
  
    sender = self.sender()
    self.statusBar().showMessage(sender.text() + ' was pressed')
```

通过调用 `sender()` 方法来确定信号源。在状态栏中显示相关信息。

![Event sender](http://zetcode.com/img/gui/pyside/sender.png)

## 发射信号

通过 QtCore.QObject 创建的对象可以发生信号。点击按钮，将产生一个点击信号。在下面的例子中，将发生一个自定义信号。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we show how to emit a
signal. 

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui, QtCore

class Communicate(QtCore.QObject):
    
    closeApp = QtCore.Signal()

class Example(QtGui.QMainWindow):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):      

        self.c = Communicate()
        self.c.closeApp.connect(self.close)       
        
        self.setGeometry(300, 300, 290, 150)
        self.setWindowTitle('Emit signal')
        self.show()
        
    def mousePressEvent(self, event):
        
        self.c.closeApp.emit()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

我们创建一个叫做 closeApp 的信号。鼠标按下的同时发射该信号，并连接到 QtGui.QMainWindow 的 `close()` 方法。

```python
class Communicate(QtCore.QObject):
    
    closeApp = QtCore.Signal()
```

创建一个基于 QtCore.QObject 的新类，当该类实例化的时候将创建一个 closeApp 信号。

```python
self.c = Communicate()
self.c.closeApp.connect(self.close)     
```

第一行创建一个 Communicate 类的实例，第二行连接 closeApp 信号到 QtGui.QMainWindow 的 close() 方法。

```python
def mousePressEvent(self, event):
    
    self.c.closeApp.emit()
```

点击主窗体的任何位置，closeApp 信号将发射，结果是窗体关闭。

在本章，我们学习了信号和时隙。


