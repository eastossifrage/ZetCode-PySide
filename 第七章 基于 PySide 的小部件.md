# 第七章 基于 PySide 的小部件

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

窗体小部件是 GUI 应用程序的基本组成部分。PySide 有各种各样的小部件，按钮，复选框，滑块，列表框等，为程序员提供了所需要的一切。在本教程的这一节中，我们将描述几个常用的小部件 —— QtGui.QCheckBox，ToggleButton，QtGui.QSlider，QtGui.QProgressBar 和 QtGui.QCalendarWidget。

## QtGui.QCheckBox 复选框小部件

QtGui.QCheckBox 有两个状态：开和关，并且带有一个标签。复选框通常用于表示应用程序中可以启用或禁用而不影响其他应用程序的功能。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, a QtGui.QCheckBox widget
is used to toggle the title of a window.

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

        cb = QtGui.QCheckBox('Show title', self)
        cb.move(20, 20)
        cb.toggle()
        cb.stateChanged.connect(self.changeTitle)
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('QtGui.QCheckBox')
        self.show()
        
    def changeTitle(self, state):
      
        if state == QtCore.Qt.Checked:
            self.setWindowTitle('Checkbox')
        else:
            self.setWindowTitle('')
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例中，将创建一个复选框来切换窗体标题。

```python
cb = QtGui.QCheckBox('Show title', self)
```

这是 QtGui.QCheckBox 构造函数。

```python
cb.toggle()
```

要设置窗口标题，必须选中复选框。

```python
cb.stateChanged.connect(self.changeTitle)
```

将 statChanged 信号与自定义的 `changeTItle()` 方法绑定在一起，可切换窗体标题。

```python
def changeTitle(self, state):
  
    if state == QtCore.Qt.Checked:
        self.setWindowTitle('Checkbox')
    else:
        self.setWindowTitle('')
```

通过 state 参数接受复选框的状态，如果选中，则改变窗体标题，否则，则标题为空字符串。

![CheckBox](http://zetcode.com/img/gui/pyside/checkbox.png)

## ToggleButton

ToggleButton 是一个有两个状态的按钮——是否按下，可通过点击该按钮在两个状态之间进行切换。PySide 没有用于 ToggleButton 的窗体部件。要创建 ToggleButton，可以在特殊模式下使用 QtGui.QPushButton。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we create three toggle buttons.
They will control the background color of a 
QtGui.QFrame. 

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

        self.col = QtGui.QColor(0, 0, 0)       

        redb = QtGui.QPushButton('Red', self)
        redb.setCheckable(True)
        redb.move(10, 10)

        redb.clicked[bool].connect(self.setColor)

        greenb = QtGui.QPushButton('Green', self)
        greenb.setCheckable(True)
        greenb.move(10, 60)

        greenb.clicked[bool].connect(self.setColor)

        blueb = QtGui.QPushButton('Blue', self)
        blueb.setCheckable(True)
        blueb.move(10, 110)

        blueb.clicked[bool].connect(self.setColor)

        self.square = QtGui.QFrame(self)
        self.square.setGeometry(150, 20, 100, 100)
        self.square.setStyleSheet("QWidget { background-color: %s }" %  
            self.col.name())
        
        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('Toggle button')
        self.show()
        
    def setColor(self, pressed):
        
        source = self.sender()
        
        if pressed:
            val = 255
        else: val = 0
                        
        if source.text() == "Red":
            self.col.setRed(val)                
        elif source.text() == "Green":
            self.col.setGreen(val)             
        else:
            self.col.setBlue(val) 
            
        self.square.setStyleSheet("QFrame { background-color: %s }" %
            self.col.name())  
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在本例中，创建有三个 ToggleButton 和一个 背景颜色默认为黑色的 QtGui.QFrame 小部件。togglebuttons 将切换颜色值的红色，绿色和蓝色部分。 QtGui.QFrame 小部件的颜色将取决于我们按下了哪个 togglebuttons。

```python
self.col = QtGui.QColor(0, 0, 0)  
```

设置默认颜色为黑色。

```python
greenb = QtGui.QPushButton('Green', self)
greenb.setCheckable(True)
```
创建通过 QtGui.QPushButton 创建一个可切换按钮，并通过 `setCheckeable()` 方法使其可选择状态。

```python
greenb.clicked[bool].connect(self.setColor)
```

绑定 clicked[bool] 信号到自定义的方法。注意，该信号发送的是一个布尔参数，其状态依赖于按钮的状态。

```python
source = self.sender()
```

接收到绑定到切换按钮的状态信息的信号。

```python
if source.text() == "Red":
    self.col.setRed(val)   
```

如果按下的是红色按钮，我们相应地更新颜色的红色部分。

```python
self.square.setStyleSheet("QFrame { background-color: %s }" %
    self.col.name())
```

使用样式表来更改 QtGui.QFrame 窗体小部件的背景颜色。

![ToggleButton](http://zetcode.com/img/gui/pyside/togglebutton.png)

## QtGui.QSlider

QtGui.QSlider 是一个具有简单手柄的小部件， 此手柄可以来回拉动，通过该方式，可为特定的人物选取一个值。QtGui.QLabel 用于显示文本或图像。

本示例中，将显示一个滑块和一个标签。标签将显示图像。滑块将控制标签。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows a QtGui.QSlider widget.

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

        sld = QtGui.QSlider(QtCore.Qt.Horizontal, self)
        sld.setFocusPolicy(QtCore.Qt.NoFocus)
        sld.setGeometry(30, 40, 100, 30)
        sld.valueChanged[int].connect(self.changeValue)
        
        self.label = QtGui.QLabel(self)
        self.label.setPixmap(QtGui.QPixmap('mute.png'))
        self.label.setGeometry(160, 40, 80, 30)
        
        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('QtGui.QSlider')
        self.show()
        
    def changeValue(self, value):

        if value == 0:
            self.label.setPixmap(QtGui.QPixmap('mute.png'))
        elif value > 0 and value <= 30:
            self.label.setPixmap(QtGui.QPixmap('min.png'))
        elif value > 30 and value < 80:
            self.label.setPixmap(QtGui.QPixmap('med.png'))
        else:
            self.label.setPixmap(QtGui.QPixmap('max.png'))
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例是模拟音量控制。通过拖动滑块的手柄，将更改标签上的图像。

```python
sld = QtGui.QSlider(QtCore.Qt.Horizontal, self)
```

创建一个水平的 QtGui.QSlider。

```python
self.label = QtGui.QLabel(self)
self.label.setPixmap(QtGui.QPixmap('mute.png'))
```

创建 a QtGui.QLabel 向部件，并设置默认的显示图片。


```python
sld.valueChanged[int].connect(self.changeValue)
```

绑定 valueChanged[int] 信号到自动以的 `changeValue()` 方法。

```python
if value == 0:
    self.label.setPixmap(QtGui.QPixmap('mute.png'))
...
```

根据滑块的值我们为标签设置不同的显示图片，上述代码是在滑块的值为 0 时，设置 `mute.png` 为显示图片。

![QSlider](http://zetcode.com/img/gui/pyside/slider.png)

## QtGui.QProgressBar

进度条是当用户处理冗长的任务时使用的一个小部件。 它是动画的，以便用户知道当前任务正在进行。QtGui.QProgressBar 小部件提供了一个水平或垂直的进度条。程序员可以设置进度条的最小值和最大值。 默认值为 0 - 99 。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows a QtGui.QProgressBar widget.

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

        self.pbar = QtGui.QProgressBar(self)
        self.pbar.setGeometry(30, 40, 200, 25)

        self.btn = QtGui.QPushButton('Start', self)
        self.btn.move(40, 80)
        self.btn.clicked.connect(self.doAction)

        self.timer = QtCore.QBasicTimer()
        self.step = 0
        
        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('QtGui.QProgressBar')
        self.show()
        
    def timerEvent(self, e):
      
        if self.step >= 100:
            self.timer.stop()
            self.btn.setText('Finished')
            return
        self.step = self.step + 1
        self.pbar.setValue(self.step)

    def doAction(self):
      
        if self.timer.isActive():
            self.timer.stop()
            self.btn.setText('Start')
        else:
            self.timer.start(100, self)
            self.btn.setText('Stop')
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例中，有一个水平进度条和一个按钮，按钮可启动和停止进度条。

```python
self.pbar = QtGui.QProgressBar(self)
```

这是一个 QtGui.QProgressBar 构造函数。

```python
self.timer = QtCore.QBasicTimer()
```

使用timer对象来激活进度条。

```python
self.timer.start(100, self)
```

调用 `start()` 方法来启动定时器事件。该方法有两个参数——超时时间和接收事件的对象。

```python
def timerEvent(self, e):
  
    if self.step >= 100:
        self.timer.stop()
        self.btn.setText('Finished')
        return
    self.step = self.step + 1
    self.pbar.setValue(self.step)
```

每个 QtCore.QObject 及其后代都有一个 `timerEvent()` 事件句柄。为了对定时器事件做出反应，此处重新实现了事件句柄——更新 self.step 变量并为进度条设置一个新值。

```python
def doAction(self):
  
    if self.timer.isActive():
        self.timer.stop()
        self.btn.setText('Start')
    else:
        self.timer.start(100, self)
        self.btn.setText('Stop')
```

定义 doAction()` 方法，启动或停止定时器。

![ProgressBar](http://zetcode.com/img/gui/pyside/progressbar.png)

## QtGui.QCalendarWidget

QtGui.QCalendarWidget 提供每月的日历小部件, 它允许用户以简单直观的方式选择日期。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows a QtGui.QCalendarWidget widget.

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

        cal = QtGui.QCalendarWidget(self)
        cal.setGridVisible(True)
        cal.move(20, 20)
        cal.clicked[QtCore.QDate].connect(self.showDate)
        
        self.lbl = QtGui.QLabel(self)
        date = cal.selectedDate()
        self.lbl.setText(date.toString())
        self.lbl.move(130, 260)
        
        self.setGeometry(300, 300, 350, 300)
        self.setWindowTitle('Calendar')
        self.show()
        
    def showDate(self, date):     
        self.lbl.setText(date.toString())
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本示例具有日历小部件和标签小部件。当前选择的日期显示在标签窗体小部件中。

```python
self.cal = QtGui.QCalendarWidget(self)
```

创建一个日历小部件。

```python
cal.clicked[QtCore.QDate].connect(self.showDate)
```

如果用户从窗口小部件中选择一个日期，则会发出一个点击的[QtCore.QDate]信号，此处将该信号绑定到自定义的 `showDate()` 方法。

```python
def showDate(self, date):     
    self.lbl.setText(date.toString())
```

调用 `selectedDate()` 方法取回所选择的日期值，需要将日期对象转换为字符串并在标签窗体小部件中显示。

![Calendar](http://zetcode.com/img/gui/pyside/calendar.png)





