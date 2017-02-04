# 第十一章 基于 PySide 自定义小部件

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

没有哪个工具包能够满足程序员所有的需求。PySide 虽提供丰富的小部件，但也是常见的几种 —— 如按钮，文本小部件，滑块等。如果需要更专门的小部件，必须自己创建它。

自定义小部件是通过使用工具包提供的绘图工具来创建的。程序员可以修改或优化现有的小部件，或者、可以从头创建一个自定义的窗体部件。

## 显示刻录进度的小部件

这个小部件可以在 Nero，K3B或其它 CD / DVD 刻录软件中看到，但是，这不是在 PySide 工具包中预定义好的小部件，需要程序员基于最小的 QtGui.QWidget 小部件从头开始创建。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we create a custom widget.

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys
from PySide import QtGui, QtCore

class Communicate(QtCore.QObject):
    
    updateBW = QtCore.Signal(int)

class BurningWidget(QtGui.QWidget):
  
    def __init__(self):      
        super(BurningWidget, self).__init__()
        
        self.initUI()
        
    def initUI(self):
        
        self.setMinimumSize(1, 30)
        self.value = 75
        self.num = [75, 150, 225, 300, 375, 450, 525, 600, 675]        


    def setValue(self, value):

        self.value = value


    def paintEvent(self, e):
      
        qp = QtGui.QPainter()
        qp.begin(self)
        self.drawWidget(qp)
        qp.end()
      
      
    def drawWidget(self, qp):
      
        font = QtGui.QFont('Serif', 7, QtGui.QFont.Light)
        qp.setFont(font)

        size = self.size()
        w = size.width()
        h = size.height()

        step = int(round(w / 10.0))


        till = int(((w / 750.0) * self.value))
        full = int(((w / 750.0) * 700))

        if self.value >= 700:
            qp.setPen(QtGui.QColor(255, 255, 255))
            qp.setBrush(QtGui.QColor(255, 255, 184))
            qp.drawRect(0, 0, full, h)
            qp.setPen(QtGui.QColor(255, 175, 175))
            qp.setBrush(QtGui.QColor(255, 175, 175))
            qp.drawRect(full, 0, till-full, h)
        else:
            qp.setPen(QtGui.QColor(255, 255, 255))
            qp.setBrush(QtGui.QColor(255, 255, 184))
            qp.drawRect(0, 0, till, h)


        pen = QtGui.QPen(QtGui.QColor(20, 20, 20), 1, 
            QtCore.Qt.SolidLine)
            
        qp.setPen(pen)
        qp.setBrush(QtCore.Qt.NoBrush)
        qp.drawRect(0, 0, w-1, h-1)

        j = 0

        for i in range(step, 10*step, step):
          
            qp.drawLine(i, 0, i, 5)
            metrics = qp.fontMetrics()
            fw = metrics.width(str(self.num[j]))
            qp.drawText(i-fw/2, h/2, str(self.num[j]))
            j = j + 1

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):      

        sld = QtGui.QSlider(QtCore.Qt.Horizontal, self)
        sld.setFocusPolicy(QtCore.Qt.NoFocus)
        sld.setRange(1, 750)
        sld.setValue(75)
        sld.setGeometry(30, 40, 150, 30)

        self.c = Communicate()
        self.wid = BurningWidget()
        self.c.updateBW[int].connect(self.wid.setValue)        

        sld.valueChanged[int].connect(self.changeValue)
        hbox = QtGui.QHBoxLayout()
        hbox.addWidget(self.wid)
        vbox = QtGui.QVBoxLayout()
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
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中，有一个 QtGui.QSlider 滑块和一个自定义的小部件，滑块用来控制自定义窗体的界面显示，此小部件以图形方式显示介质的总容量和可用空间。自定义窗体部件的最小值是1，最大值是750. 如果达到值700，开始显示出红色，通常表示过度刻录。

显示刻录进度的小部件放置在主窗体的底部，使用了一个 QtGui.QHBoxLayout 和一个 QtGui.QVBoxLayout。

```python
class BurningWidget(QtGui.QWidget):
  
    def __init__(self):      
        super(BurningWidget, self).__init__()
```

刻录小部件继承与 QtGui.QWidget widget。

```python
self.setMinimumSize(1, 30)
```

设置小部件的最小尺寸（高度）。

```python
font = QtGui.QFont('Serif', 7, QtGui.QFont.Light)
qp.setFont(font)
```

设置字体的样式和大小。

```python
size = self.size()
w = size.width()
h = size.height()

step = int(round(w / 10.0))


till = int(((w / 750.0) * self.value))
full = int(((w / 750.0) * 700))

```

上述代码是在绘制一个动态代码。刻录进度的小部件会随着主窗体的大小进行相应的变化。till 参数用来确定要绘制的图画的大小，此值来自滑块小部件，它是整个面积的一个比例值。full 参数确定点，在这里开始绘制红色，注意使用浮点运算。

实际的绘图分为三个步骤，首先，画出黄色或红色的矩形，然后，绘制将刻录进度小部件分为几部分的垂直线，最后，画出表示介质容量的数字。

```python
metrics = qp.fontMetrics()
fw = metrics.width(str(self.num[j]))
qp.drawText(i-fw/2, h/2, str(self.num[j]))
```

依据文字的大小来绘制文本。首先要知道文本的宽度，以使其围绕垂直线居中。

```python
def changeValue(self, value):
          
    self.c.updateBW.emit(value)
    self.wid.repaint()
```

滑动滑块，将调用 `call()` 方法。本方法内，发送一个带有参数的自定义 updateBW 信号，该参数是滑块的当前值，用于计算要绘制的刻录进度窗体小部件的容量，然后重新绘制自定义窗体小部件。

![Burning](http://zetcode.com/img/gui/pyside/customwidget.png)






