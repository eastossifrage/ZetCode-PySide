# 第二章 基于 PySide 的第一个程序

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。
本章主要讲解一些 PySide 的基本功能。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

## 简单实例

本代码实例非常的简单。仅仅是展示了一个小窗口。当然，我们可以在这个小窗口的基础上，做很多其它的扩展。比如我们可以调整窗口的大小，最大化或最小化，这通常需要大量的代码，但是由于这些应用在大量的程序中都需要，重新编写代码，会显得重复和低效，为了解决这样的问题，有人已经编写了这些功能 —— PySide，程序员无需再重新造轮子。PySide 是一个高阶工具包，下面的代码只要简单的几时行即可。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

# simple.py

import sys
from PySide import QtGui

app = QtGui.QApplication(sys.argv)

wid = QtGui.QWidget()
wid.resize(250, 150)
wid.setWindowTitle('Simple')
wid.show()

sys.exit(app.exec_())
```

上面的代码可以在屏幕上显示一个小窗体。

```python
import sys
from PySide import QtGui
```

在这里需要导入必须的模块，最基本的 GUI 小部件（widgets）包含在 QtGui 模块中。

```python
app = QtGui.QApplication(sys.argv)
```

每一个 PySide 程序必须创建一个程序对象，该对象包含在 QtGui 模块中。 `sys.argv` 参数是一个来自于命令行的数组，Python脚本可以从shell运行，通过这种方式，我们可以控制我们的脚本启动。

```python
wid = QtGui.QWidget()
```

QWidget 是PySide中所有用户界面对象的基类。此处我们提供没有父类的 QWidget 的默认构造函数，其返回一个窗体。

```python
wid.resize(250, 150)
```

通过 `resize()` 方法，可以设置窗体的大小。此处是 250px 宽和 150px 高。

```python
wid.setWindowTitle('Simple')
```

此处代码是为窗体设置标题，将在窗体的标题栏里显示。

```python
wid.show()
```

`show()` 方法用来在屏幕上显示窗体小部件。

```python
sys.exit(app.exec_())
```

最后，我们进入程序的主循环（mainloop），事件处理也将从此点开始。主循环（mainloop）从窗体系统（window system）中接收事件并分派到相应的程序小部件。如果我们调用 `exit()` 方法或主体窗体被销毁，主循环（mainloop）结束。`sys.exit()` 方法确保程序能够干干净净的退出。

*你向知道 `exec_()` 方法为什么会有一个下划线吗？* 这显然是因为 exec 是 python 的一个关键字。使用下划线也是为了区别于关键字。

![Simple](http://zetcode.com/img/gui/pyside/simple.png)

## 应用程序图标

应用程序图标是一个小图片，通常显示在标题栏的左上角，在任务栏中也是可见的。下面的例子，我们将展示如果在 PySide 的实例中创建图标，同时也将介绍一些新的方法。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows an icon
in the titlebar of the window.

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
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Icon')
        self.setWindowIcon(QtGui.QIcon('web.png'))        
    
        self.show()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

前面的例子是以过程化风格编码的。Python支持过程化和面向对象的编程风格。PySide 编程建议使用面向对象的编程风格。

```python
class Example(QtGui.QWidget):
    
    def __init__(self):
        super(Example, self).__init__()
```

面向对象编程中最重要的三件事是类、数据和方法。本实例中我们创建了一个 Example 类，其继承于 QtGui.QWidget 类。这意味着我们必须调用两个构造函数。第一个为Example类，第二个为所继承的父类。第二个用super（）方法调用。

```python
self.setGeometry(300, 300, 250, 150)
self.setWindowTitle('Icon')
self.setWindowIcon(QtGui.QIcon('web.png')) 
```

这三个方法都是继承于父类 QtGui.QWidget。`setGeometry()` 方法做了两件事，在屏幕上定位窗体的位置和设置大小。前两个参数是窗体的 x 轴和 y 轴的坐标位置。第三个参数是窗体宽度，第四个参数是窗体高度。最后一个方法是用来设置应用程序的图标，为了实现该功能，需要创建一个 QtGui.QIcon 对象，接受要显示的图标的路径。

```python
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

我们把启动代码放在main（）方法里面。这是 python 的惯用语法。

![icon](http://zetcode.com/img/gui/pyside/icon.png)

一个图标显示在了窗体的左上角。

## 显示提示（tooltip）

我们可以为窗体的任何小部件提供气泡式的提示帮助。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This example shows a tooltip on 
a window and a button

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
        
        QtGui.QToolTip.setFont(QtGui.QFont('SansSerif', 10))
        
        self.setToolTip('This is a <b>QWidget</b> widget')
        
        btn = QtGui.QPushButton('Button', self)
        btn.setToolTip('This is a <b>QPushButton</b> widget')
        btn.resize(btn.sizeHint())
        btn.move(50, 50)       
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Tooltips')    
        self.show()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在该实例中，我们为两个 PySide 的小部件显示“提示”效果。

```python
QtGui.QToolTip.setFont(QtGui.QFont('SansSerif', 10))
```

这个静态方法设置用于呈现“提示”的字体，使用10px的SansSerif字体。

```python
self.setToolTip('This is a <b>QWidget</b> widget')
```

此处调用 `setTooltip()` 方法来创建“提示”。并使用 HTML 标签语言来格式化所要显示的提示内容。

```python
btn = QtGui.QPushButton('Button', self)
btn.setToolTip('This is a <b>QPushButton</b> widget')
```

此处创建按钮小部件，并为其设置了提示内容。

```python
btn.resize(btn.sizeHint())
btn.move(50, 50)   
```

此处是用来设置按钮的大小以及在窗体中的位置。`sizeHint()` 方法为按钮设置了自适应的大小。

![tooltips](http://zetcode.com/img/gui/pyside/tooltips.png)

## 关闭窗体

关闭窗口的明显方法是点击标题栏上的 x 标记。在下面的例子中，我们将展示如果通过编程来关闭一个窗体。我们将简略地触摸信号和时隙的概念。

下面是一个QtGui.QPushButton的构造体类，我们将在我们的示例中使用。

```python
class PySide.QtGui.QPushButton(text[, parent=None])
```

text 参数是将在按钮上显示的内容。parent 参数是放置按钮的小部件。在本例中，是`QtGui.QWidget`。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This program creates a quit
button. When we press the button,
the application terminates. 

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
        
        qbtn = QtGui.QPushButton('Quit', self)
        qbtn.clicked.connect(QtCore.QCoreApplication.instance().quit)
        qbtn.resize(qbtn.sizeHint())
        qbtn.move(50, 50)       
        
        self.setGeometry(300, 300, 250, 150)
        self.setWindowTitle('Quit button')    
        self.show()
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在本例中，我们创建一个退出按钮。点击按钮后，应用程序将终止运行。

```python
qbtn = QtGui.QPushButton('Quit', self)
```

此处我们创建一个按钮。 该按钮是 QtGui.QPushButton 类的一个实例。 第一个参数是按钮的标签， 第二个参数是父窗体小部件， 父窗体部件是 Example 窗体部件，它是一个继承于 QtGui.QWidget 的类。

```python
qbtn.clicked.connect(QtCore.QCoreApplication.instance().quit)
```

PySide 中的事件处理系统是用信号和时隙机制构建的。如果点击按钮，点击的信号就会被发射，时隙可以是 Qt 时隙或任何 python 的回调。QtCore.QCoreApplication包含主要事件循环，它处理和分派所有事件。`instance（）`方法为我们提供了当前的实例。注意，QtCore.QCoreApplication 是由使用 QtGui.QApplication 创建的 qbtn 实例调用的，点击按钮的信号连接到 `quit（）`方法，它将终止应用程序。通信是在两个对象之间完成的，发送方和接收方，发送方是按钮，接收方是应用程序对象。

![quitButton](http://zetcode.com/img/gui/pyside/quitbutton.png)

## 消息框

默认情况下，如果我们点击标题栏上的 x 按钮，QtGui.QWidget 窗体将关闭。 有时我们想修改这个默认行为。 例如，如果我们在编辑器中打开了一个文件，我们对其进行了一些更改。 我们显示一个消息框以确认保存修改操作。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This program shows a confirmation 
message box when we click on the close
button of the application window. 

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
        
        self.setGeometry(300, 300, 250, 150)        
        self.setWindowTitle('Message box')    
        self.show()
        
    def closeEvent(self, event):
        
        reply = QtGui.QMessageBox.question(self, 'Message',
            "Are you sure to quit?", QtGui.QMessageBox.Yes | 
            QtGui.QMessageBox.No, QtGui.QMessageBox.No)

        if reply == QtGui.QMessageBox.Yes:
            event.accept()
        else:
            event.ignore()        
        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

如果关闭 QtGui.QWidget 窗体，则会生成 QCloseEvent 事件信号。 要修改窗口小部件的行为，需要重新实现 `closeEvent（）`事件处理程序。

```python
reply = QtGui.QMessageBox.question(self, 'Message',
    "Are you sure to quit?", QtGui.QMessageBox.Yes | 
    QtGui.QMessageBox.No, QtGui.QMessageBox.No)
```

消息框上将显示两个按钮——是和否。第一个参数的内容将出现在标题栏上，第二个参数的内容是对框框显示的消息内容，第三个参数是对话框中出现的按钮组合，最后一个参数是默认按钮，也是当前键盘的默认焦点。返回值存储在 reply 变量中。

```python
if reply == QtGui.QMessageBox.Yes:
    event.accept()
else:
    event.ignore() 
```

这里我们检测返回值的内容。如果我们单击了 “Yes” 按钮，将会接受导致窗体关闭和应用程序终止的事件，否则，将接受忽略事件。

![messageBox](http://zetcode.com/img/gui/pyside/messagebox.png)

## 屏幕居中显示

以下脚本展示了如何在屏幕上居中显示窗体。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

This program centers a window 
on the screen. 

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
        
        self.resize(250, 150)
        self.center()
        
        self.setWindowTitle('Center')    
        self.show()
        
    def center(self):
        
        qr = self.frameGeometry()
        cp = QtGui.QDesktopWidget().availableGeometry().center()
        qr.moveCenter(cp)
        self.move(qr.topLeft())
        
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

在屏幕中居中显示窗体

```python
self.center()
```

将窗体居中的代码放在自定义center（）方法中。

```python
qr = self.frameGeometry()
```

此处，我们得到了一个具有几何学特性的矩形，此特性包含了所有的窗体控件。

```python
cp = QtGui.QDesktopWidget().availableGeometry().center()
```

依据显示器的屏幕分辨率，得到的中心点。

```python
qr.moveCenter(cp)
```

窗体矩形本身具有宽度和高度，而且大小保持不变，现在将矩形的中心设置为屏幕中心。

```python
self.move(qr.topLeft())
```

将应用程序窗体的左上点移动到 qr 矩形的左上点，从而将窗体在屏幕上居中显示。









