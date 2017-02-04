# 第十章 PySide 绘图

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

有时，为了改变或优化现有的小部件，需要我们从头开始构建自定义的小部件，这时，可使用 PySide 工具包提供的绘图 API。

绘图代码将在 `paintEvent()` 方法内完成，放置在 QtGui.QPainter 对象的`begin()` 和 `end()` 方法之间。

## 绘制文本

开始绘制一些 Unicode 文本到窗体客户区。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we draw text in Russian azbuka.

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

        self.text = u'\u041b\u0435\u0432 \u041d\u0438\u043a\u043e\u043b\u0430\
\u0435\u0432\u0438\u0447 \u0422\u043e\u043b\u0441\u0442\u043e\u0439: \n\
\u0410\u043d\u043d\u0430 \u041a\u0430\u0440\u0435\u043d\u0438\u043d\u0430'

        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('Draw text')
        self.show()

    def paintEvent(self, event):

        qp = QtGui.QPainter()
        qp.begin(self)
        self.drawText(event, qp)
        qp.end()
        
    def drawText(self, event, qp):
      
        qp.setPen(QtGui.QColor(168, 34, 3))
        qp.setFont(QtGui.QFont('Decorative', 10))
        qp.drawText(event.rect(), QtCore.Qt.AlignCenter, self.text)        
                
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在本示例中，使用俄语 Azbuka 绘制一些水平和垂直对齐的文本。

```python
def paintEvent(self, event):
```
利用绘画事件来进行绘图。

```python
qp = QtGui.QPainter()
qp.begin(self)
self.drawText(event, qp)
qp.end()
```

QtGui.QPainter 类负责所有的低阶绘画。所有的绘制代码都写在 `begin()` 和 `end()` 方法之间。 实际绘画被委托给 `drawText()` 方法。

```python
qp.setPen(QtGui.QColor(168, 34, 3))
qp.setFont(QtGui.QFont('Decorative', 10))
```

此处，我们定义了用来绘制文本的笔和字体。

```python
qp.drawText(event.rect(), QtCore.Qt.AlignCenter, self.text)
```

`drawText()` 方法在窗体上绘制文本。绘画事件的 `rect()` 法返回需要更新的矩形。

![Drawing Text](http://zetcode.com/img/gui/pyside/drawtext.png)

## 绘制小点

点是可以绘制的最简单的图形对象。它是在窗体上的一个小斑点。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In the example, we draw randomly 1000 red points 
on the window.

author: Jan Bodnar
website: zetcode.com 
last edited: August 2011
"""

import sys, random
from PySide import QtGui, QtCore

class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):      

        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('Points')
        self.show()

    def paintEvent(self, e):

        qp = QtGui.QPainter()
        qp.begin(self)
        self.drawPoints(qp)
        qp.end()
        
    def drawPoints(self, qp):
      
        qp.setPen(QtCore.Qt.red)
        size = self.size()
        
        for i in range(1000):
            x = random.randint(1, size.width()-1)
            y = random.randint(1, size.height()-1)
            qp.drawPoint(x, y)     
                
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在本例中，我们在客户区上随即绘制了 1000 个红点。

```python
qp.setPen(QtCore.Qt.red)
```

使用预定义的颜色常量将绘笔设置为红色。

```python
size = self.size()
```

使用 `size()` 方法获取窗体的当前大小，作为小点在当前窗体随机分布的依据。

```python
qp.drawPoint(x, y) 
```

使用 `drawPoint()` 方法进行绘图。

![Points](http://zetcode.com/img/gui/pyside/points.png)

## 颜色

计算机中的颜色是指红色，绿色和蓝色（RGB）值相互组合的对象。有效的RGB值在 0 到 255 的范围内。定义颜色值有多种方式，最常见的是RGB十进制值或十六进制值。还可以使用 RGBA 值，它代表红色，绿色，蓝色，阿尔法——透明度。Alpha 值为 255 表示完全不透明度，0 表示完全透明度，颜色是不可见的。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example draws three rectangles in three
different colors. 

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

        self.setGeometry(300, 300, 350, 100)
        self.setWindowTitle('Colors')
        self.show()

    def paintEvent(self, e):

        qp = QtGui.QPainter()
        qp.begin(self)
        self.drawRectangles(qp)
        qp.end()
        
    def drawRectangles(self, qp):
      
        color = QtGui.QColor(0, 0, 0)
        color.setNamedColor('#d4d4d4')
        qp.setPen(color)

        qp.setBrush(QtGui.QColor(200, 0, 0))
        qp.drawRect(10, 15, 90, 60)

        qp.setBrush(QtGui.QColor(255, 80, 0, 160))
        qp.drawRect(130, 15, 90, 60)

        qp.setBrush(QtGui.QColor(25, 0, 90, 200))
        qp.drawRect(250, 15, 90, 60)
              
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中，绘制了三个有颜色的矩形。

```python
color = QtGui.QColor(0, 0, 0)
color.setNamedColor('#d4d4d4')
```

此处使用十六进制符号定义颜色。

```python
qp.setPen(color)
```

定义绘笔的颜色，用于绘制轮廓。

```python
qp.setBrush(QtGui.QColor(200, 0, 0))
qp.drawRect(10, 15, 90, 60)
```

此处，定义绘笔并绘制一个矩形，同时添加背景颜色。`drawRect()` 方法接受四个参数。 前两个是坐标轴上的x，y值。 第三和第四个参数是矩形的宽度和高度。

![Color](http://zetcode.com/img/gui/pyside/colors.png)

## QtGui.QPen

QtGui.QPen 是一个基本的图形对象。 它用于绘制矩形，椭圆，多边形或其他形状的线，曲线和轮廓。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example draws three rectangles in three
different colors. 

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

        self.setGeometry(300, 300, 280, 270)
        self.setWindowTitle('Pen styles')
        self.show()

    def paintEvent(self, e):

        qp = QtGui.QPainter()
        qp.begin(self)
        self.drawLines(qp)
        qp.end()
        
    def drawLines(self, qp):
      
        pen = QtGui.QPen(QtCore.Qt.black, 2, QtCore.Qt.SolidLine)

        qp.setPen(pen)
        qp.drawLine(20, 40, 250, 40)

        pen.setStyle(QtCore.Qt.DashLine)
        qp.setPen(pen)
        qp.drawLine(20, 80, 250, 80)

        pen.setStyle(QtCore.Qt.DashDotLine)
        qp.setPen(pen)
        qp.drawLine(20, 120, 250, 120)

        pen.setStyle(QtCore.Qt.DotLine)
        qp.setPen(pen)
        qp.drawLine(20, 160, 250, 160)

        pen.setStyle(QtCore.Qt.DashDotDotLine)
        qp.setPen(pen)
        qp.drawLine(20, 200, 250, 200)

        pen.setStyle(QtCore.Qt.CustomDashLine)
        pen.setDashPattern([1, 4, 5, 4])
        qp.setPen(pen)
        qp.drawLine(20, 240, 250, 240)
              
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中，画出了六种不同样式的线条。有五种是使用 PySide 工具包里预定义的样式，最后一行是程序员自定义的样式。

```python
pen = QtGui.QPen(QtCore.Qt.black, 2, QtCore.Qt.SolidLine)
```

创建一个 QtGui.QPen 对象，其颜色是黑色的，宽度设置为2像素，主要是为了体现不同样式之间的差异。 QtCore.Qt.SolidLine 是预定义的样式之一。

```python
pen.setStyle(QtCore.Qt.CustomDashLine)
pen.setDashPattern([1, 4, 5, 4])
qp.setPen(pen)
```

此处自定义了一种样式。调用 `setDashPattern()` 方法。数字列表定义了一个样式，必须有偶数个数字。奇数定义了破折号，偶数数字空格。数字越大，空格或破折号就越大。 此处的模式是1px dash 4px space 5px dash 4px space等。

![Pen Styles](http://zetcode.com/img/gui/pyside/penstyles.png)

## QtGui.QBrush

QtGui.QBrush 是一个基本的图形对象。它用于绘制图形形状的背景，如矩形，椭圆或多边形。刷子一般有三种不同类型。预定义的笔刷渐变或纹理图案。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example draws 9 rectangles in different
brush styles.

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

        self.setGeometry(300, 300, 355, 280)
        self.setWindowTitle('Brushes')
        self.show()

    def paintEvent(self, e):

        qp = QtGui.QPainter()
        qp.begin(self)
        self.drawBrushes(qp)
        qp.end()
        
    def drawBrushes(self, qp):
      
        brush = QtGui.QBrush(QtCore.Qt.SolidPattern)
        qp.setBrush(brush)
        qp.drawRect(10, 15, 90, 60)

        brush.setStyle(QtCore.Qt.Dense1Pattern)
        qp.setBrush(brush)
        qp.drawRect(130, 15, 90, 60)

        brush.setStyle(QtCore.Qt.Dense2Pattern)
        qp.setBrush(brush)
        qp.drawRect(250, 15, 90, 60)

        brush.setStyle(QtCore.Qt.Dense3Pattern)
        qp.setBrush(brush)
        qp.drawRect(10, 105, 90, 60)

        brush.setStyle(QtCore.Qt.DiagCrossPattern)
        qp.setBrush(brush)
        qp.drawRect(10, 105, 90, 60)

        brush.setStyle(QtCore.Qt.Dense5Pattern)
        qp.setBrush(brush)
        qp.drawRect(130, 105, 90, 60)

        brush.setStyle(QtCore.Qt.Dense6Pattern)
        qp.setBrush(brush)
        qp.drawRect(250, 105, 90, 60)

        brush.setStyle(QtCore.Qt.HorPattern)
        qp.setBrush(brush)
        qp.drawRect(10, 195, 90, 60)

        brush.setStyle(QtCore.Qt.VerPattern)
        qp.setBrush(brush)
        qp.drawRect(130, 195, 90, 60)

        brush.setStyle(QtCore.Qt.BDiagPattern)
        qp.setBrush(brush)
        qp.drawRect(250, 195, 90, 60)
              
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中，使用9种不同的画笔样式来绘制九个不同的矩形。

```python
brush = QtGui.QBrush(QtCore.Qt.SolidPattern)
qp.setBrush(brush)
qp.drawRect(10, 15, 90, 60)
```

第一行定义了一个刷子对象，第二行将其设置为绘画对象，第三行使用 `drawRect()` 方法来绘制矩形。该示例为图片中的第一个矩形。

![Brushes](http://zetcode.com/img/gui/pyside/brushes.png)