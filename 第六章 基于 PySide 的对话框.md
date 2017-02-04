# 第六章 基于 PySide 的对话框

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

对话框窗体或对话框在现代GUI应用程序中非常常见。在计算机应用程序中，对话框是用于与应用程序“通话”的窗口。 对话框用于输入数据，修改数据，更改应用程序设置等，是用户和计算机程序之间交流的重要手段。

## QtGui.QInputDialog

QtGui.QInputDialog提供了一个即简单又方便的对话框用于和用户交互。输入值即可以是字符串，数字也可以是列表。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we receive data from
a QtGui.QInputDialog dialog. 

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

        self.btn = QtGui.QPushButton('Dialog', self)
        self.btn.move(20, 20)
        self.btn.clicked.connect(self.showDialog)
        
        self.le = QtGui.QLineEdit(self)
        self.le.move(130, 22)
        
        self.setGeometry(300, 300, 290, 150)
        self.setWindowTitle('Input dialog')
        self.show()
        
    def showDialog(self):
        text, ok = QtGui.QInputDialog.getText(self, 'Input Dialog', 
            'Enter your name:')
        
        if ok:
            self.le.setText(str(text))
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中有一个按钮和行编辑小部件。按钮用于获取文本值的输入对话框，输入的文本将显示在行编辑小部件中。

```python
text, ok = QtGui.QInputDialog.getText(self, 'Input Dialog', 
    'Enter your name:')
```

该代码用于显示输入对话框。 第一个参数是对话框标题，第二个参数是对话框中的消息。 对话框返回输入的文本 `text` 和布尔值 `ok` 。 如果我们单击确定按钮，布尔值将为true，否则为false。

```python
if ok:
    self.le.setText(str(text))
```
行编辑小部件将显示从对话框接收到的文本内容。

![input Dialogs](http://zetcode.com/img/gui/pyside/inputdialog.png)

## QtGui.QColorDialog

QtGui.QColorDialog 提供了一个用于选择颜色的对话框小部件。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we select a colour value
from the QtGui.QColorDialog and change the background
colour of a QtGui.QFrame widget. 

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

        col = QtGui.QColor(0, 0, 0) 

        self.btn = QtGui.QPushButton('Dialog', self)
        self.btn.move(20, 20)

        self.btn.clicked.connect(self.showDialog)

        self.frm = QtGui.QFrame(self)
        self.frm.setStyleSheet("QWidget { background-color: %s }" 
            % col.name())
        self.frm.setGeometry(130, 22, 100, 100)            
        
        self.setGeometry(300, 300, 250, 180)
        self.setWindowTitle('Color dialog')
        self.show()
        
    def showDialog(self):
      
        col = QtGui.QColorDialog.getColor()

        if col.isValid():
            self.frm.setStyleSheet("QWidget { background-color: %s }"
                % col.name())
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中将显示一个按钮和一个 QtGui.QFrame 。小部件的背景默认被设置为黑色，使用QtGui.QColorDialog，可以改变它的背景。

```python
col = QtGui.QColor(0, 0, 0) 
```

设置 QtGui.QFrame 背景的初始颜色。

```python
col = QtGui.QColorDialog.getColor()
```

弹出 QtGui.QColorDialog 对话框，用于选择颜色。

```python
if col.isValid():
    self.frm.setStyleSheet("QWidget { background-color: %s }"
        % col.name())
```

首先要判断一下用户的选择是否有效，如果点击取消按钮，则不返回有效的颜色。如果颜色有效，则更改背景颜色。

![color Dialogs](http://zetcode.com/img/gui/pyside/colordialog.png)

## QtGui.QFontDialog

QtGui.QFontDialog 是一个用于选择字体的对话框小部件。

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we select a font name
and change the font of a label. 

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

        vbox = QtGui.QVBoxLayout()

        btn = QtGui.QPushButton('Dialog', self)
        btn.setSizePolicy(QtGui.QSizePolicy.Fixed,
            QtGui.QSizePolicy.Fixed)
        
        btn.move(20, 20)

        vbox.addWidget(btn)

        btn.clicked.connect(self.showDialog)
        
        self.lbl = QtGui.QLabel('Knowledge only matters', self)
        self.lbl.move(130, 20)

        vbox.addWidget(self.lbl)
        self.setLayout(vbox)          
        
        self.setGeometry(300, 300, 250, 180)
        self.setWindowTitle('Font dialog')
        self.show()
        
    def showDialog(self):

        font, ok = QtGui.QFontDialog.getFont()
        if ok:
            self.lbl.setFont(font)
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

本例中，有一个按钮和一个标签，使用QtGui.QFontDialog，可改变标签的字体。

```python
font, ok = QtGui.QFontDialog.getFont()
```

本行用于弹出字体选择对话框。`getFont（）` 方法返回字体名称和 ok 参数。如果用户点击确定，那么 ok 参数为 True，否则为 Fasle。

```python
if ok:
    self.label.setFont(font)
```

如果 ok 参数为 True，标签的字体将被改变。

## QtGui.QFileDialog

QtGui.QFileDialog 是一个选择打开和保存文件的对话框，并且允许用户选择文件或目录。 

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

"""
ZetCode PySide tutorial 

In this example, we select a file with a
QtGui.QFileDialog and display its contents
in a QtGui.QTextEdit.

author: Jan Bodnar
website: zetcode.com 
last edited: October 2011
"""

import sys
from PySide import QtGui


class Example(QtGui.QMainWindow):
    
    def __init__(self):
        super(Example, self).__init__()
        
        self.initUI()
        
    def initUI(self):      

        self.textEdit = QtGui.QTextEdit()
        self.setCentralWidget(self.textEdit)
        self.statusBar()

        openFile = QtGui.QAction(QtGui.QIcon('open.png'), 'Open', self)
        openFile.setShortcut('Ctrl+O')
        openFile.setStatusTip('Open new File')
        openFile.triggered.connect(self.showDialog)

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(openFile)       
        
        self.setGeometry(300, 300, 350, 300)
        self.setWindowTitle('File dialog')
        self.show()
        
    def showDialog(self):

        fname, _ = QtGui.QFileDialog.getOpenFileName(self, 'Open file',
                    '/home')
        
        f = open(fname, 'r')
        
        with f:
            data = f.read()
            self.textEdit.setText(data)
                                
        
def main():
    
    app = QtGui.QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

该示例显示菜单栏、文本编辑小部件和状态栏。菜单项显示用于选择文件的 QtGui.QFileDialog 。所选文件的内容将加载到文本编辑小部件中。

```python
class Example(QtGui.QMainWindow):
  
    def __init__(self):
        super(Example, self).__init__()
```

该示例继承于 QtGui.QMainWindow 类，可以方便的创建状态栏、工具栏和居中的小窗体。

```python
fname, _ = QtGui.QFileDialog.getOpenFileName(self, 'Open file',
            '/home')
```

该处将弹出 QtGui.QFileDialog 小部件。`getOpenFileName()` 方法中的第一个参数是标题。 第二个参数指定所选文件的默认目录。 该方法返回所选文件名和过滤器，该处只对文件名感兴趣。

```python
f = open(fname, 'r')

with f:
    data = f.read()
    self.textEdit.setText(data)
```

该处代码是读取所选文件，并将内容显示在文本编辑小部件中。

