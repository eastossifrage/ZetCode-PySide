# 第八章 基于 PySide 的小部件 ||

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

本章将继续介绍PySide小部件。 主要学习 QtGui.QPixmap，QtGui.QLineEdit，QtGui.QSplitter 和 QtGui.QComboBox。

## QtGui.QPixmap

QtGui.QPixmap 是用于处理图像的小部件之一。由于优化在屏幕上显示的图像。在本代码示例中，将使用 QtGui.QPixmap 在窗口上显示图像。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we dispay an image
on the window. 

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

        hbox = QtGui.QHBoxLayout(self)
        pixmap = QtGui.QPixmap("redrock.png")

        lbl = QtGui.QLabel(self)
        lbl.setPixmap(pixmap)

        hbox.addWidget(lbl)
        self.setLayout(hbox)
        
        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('Red Rock')
        self.show()        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在本例中，使用 QtGui.QPixmap 从文件加载图像，并利用 QtGui.QLabel 小部件在窗体上显示图像。

```python
pixmap = QtGui.QPixmap("redrock.png")
```

创建 QtGui.QPixmap 对象，使用文件的名称作为参数。

```python
lbl = QtGui.QLabel(self)
lbl.setPixmap(pixmap)
```

将 pixmap 对象显示在  QtGui.QLabel 小窗体上。

## QtGui.QLineEdit

QtGui.QLineEdit 允许输入和编辑一行纯文本，有撤消/重做，剪切/粘贴和拖放功能。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows text which 
is entered in a QtGui.QLineEdit
in a QtGui.QLabel widget.
 
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

        self.lbl = QtGui.QLabel(self)
        qle = QtGui.QLineEdit(self)
        
        qle.move(60, 100)
        self.lbl.move(60, 40)

        qle.textChanged[str].connect(self.onChanged)
        
        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('QtGui.QLineEdit')
        self.show()
        
    def onChanged(self, text):
        
        self.lbl.setText(text)
        self.lbl.adjustSize()        
        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例展示了一个行编辑窗体小部件和一个标签。在行编辑中输入的内容会立即显示在标签窗体小部件中。

```python
qle = QtGui.QLineEdit(self)
```

创建 QtGui.QLineEdit 小部件。

```python
qle.textChanged[str].connect(self.onChanged)
```

如果行编辑窗体小部件的内容发生改变，会调用 `onChanged()` 方法。

```python
def onChanged(self, text):
    
    self.lbl.setText(text)
    self.lbl.adjustSize()
```

在 `onChanged()` 方法中，将输入的内容在标签窗口小部件显示。调用` adjustSize()` 方法将标签的大小调整为文本的长度。

！[QLineEdit](http://zetcode.com/img/gui/pyside/lineedit.png)

## QtGui.QSplitter

QtGui.QSplitter 允许用户通过拖动子项之间的边界来控制子组件的大小。在我们的示例中，我们显示了三个QtGui.QFrame小部件与两个拆分器。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows
how to use QtGui.QSplitter widget.
 
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

        hbox = QtGui.QHBoxLayout(self)

        topleft = QtGui.QFrame(self)
        topleft.setFrameShape(QtGui.QFrame.StyledPanel)
 
        topright = QtGui.QFrame(self)
        topright.setFrameShape(QtGui.QFrame.StyledPanel)

        bottom = QtGui.QFrame(self)
        bottom.setFrameShape(QtGui.QFrame.StyledPanel)

        splitter1 = QtGui.QSplitter(QtCore.Qt.Horizontal)
        splitter1.addWidget(topleft)
        splitter1.addWidget(topright)

        splitter2 = QtGui.QSplitter(QtCore.Qt.Vertical)
        splitter2.addWidget(splitter1)
        splitter2.addWidget(bottom)

        hbox.addWidget(splitter2)
        self.setLayout(hbox)
        QtGui.QApplication.setStyle(QtGui.QStyleFactory.create('Cleanlooks'))
        
        self.setGeometry(300, 300, 300, 200)
        self.setWindowTitle('QtGui.QSplitter')
        self.show()
        
    def onChanged(self, text):
        
        self.lbl.setText(text)
        self.lbl.adjustSize()        
        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例中，有三个框架小部件和两个分离器。

```python
topleft = QtGui.QFrame(self)
topleft.setFrameShape(QtGui.QFrame.StyledPanel)
```

该处使用一个带样式的边框，以便能够查看到 QtGui.QFrame 小部件之间的边界。

```python
splitter1 = QtGui.QSplitter(QtCore.Qt.Horizontal)
splitter1.addWidget(topleft)
splitter1.addWidget(topright)
```

创建一个 QtGui.QSplitter 小部件并添加两个边框。

```python
splitter2 = QtGui.QSplitter(QtCore.Qt.Vertical)
splitter2.addWidget(splitter1)
```

也可以添加分割器到另一个小部件。

```python
QtGui.QApplication.setStyle(QtGui.QStyleFactory.create('Cleanlooks'))
```

该处使用 Cleanlooks 样式。在其它的某些样式中，边框是不可见的。

![QSplitter](http://zetcode.com/img/gui/pyside/splitter.png)

## QtGui.QComboBox

QtGui.QComboBox 允许用户从选项列表中进行选择。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows
how to use QtGui.QComboBox widget.
 
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

        self.lbl = QtGui.QLabel("Ubuntu", self)

        combo = QtGui.QComboBox(self)
        combo.addItem("Ubuntu")
        combo.addItem("Mandriva")
        combo.addItem("Fedora")
        combo.addItem("Red Hat")
        combo.addItem("Gentoo")

        combo.move(50, 50)
        self.lbl.move(50, 150)

        combo.activated[str].connect(self.onActivated)        
         
        self.setGeometry(300, 300, 300, 200)
        self.setWindowTitle('QtGui.QComboBox')
        self.show()
        
    def onActivated(self, text):
      
        self.lbl.setText(text)
        self.lbl.adjustSize()  
                
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例显示了一个 QtGui.QComboBox 和 一个QtGui.QLabel。 组合框的下拉菜单中有五个选项，都是Linux发行版的名称。 标签窗体小部件显示从组合框的下拉菜单中选择的选项。

```python
combo = QtGui.QComboBox(self)
combo.addItem("Ubuntu")
combo.addItem("Mandriva")
combo.addItem("Fedora")
combo.addItem("Red Hat")
combo.addItem("Gentoo")
```

创建一个  QtGui.QComboBox ，并添加 5 个下拉菜单选项。

```python
combo.activated[str].connect(self.onActivated)  
```

调用`onActivated()` 方法来选择下拉菜单项。

``python
def onActivated(self, text):
  
    self.lbl.setText(text)
    self.lbl.adjustSize() 
```

该方法内，第一行是将所选下拉菜单项的文本内容在标签窗体小部件显示。第二行是调整标签的大小。

![QComboBox](http://zetcode.com/img/gui/pyside/combobox.png)
