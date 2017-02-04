# PySide工具包简介

标签（空格分隔）： python PySide

---

> 这是一个介绍 PySide 教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

## 关于 PySide

PySide 是一个基于 QT 框架的用来创建跨平台的图形用户界面的 Python 库。关于QT 库无需赘述，它是强大的 GUI 库中的一个。PySide 的官方网站是 [qt-project.org/wiki/PySide](http://qt-project.org/wiki/PySide)。官方的安装介绍是 [pypi.python.org/pypi/PySide](https://pypi.python.org/pypi/PySide)。

PySide 通过一系列 python 模块来实现。当前包含有 15 个模块。这些模块提供了强大的工具来处理GUI，多媒体，XML文档，网络或数据库。本教程中，我们将使用这些模块中的两个—— QtGui和QtCore模块。

QtCore 模块包含核心的，但是非 GUI 的功能。此模块用于处理时间，文件和目录，各种数据类型，流，URL，MIME 类型，线程或进程。QtGui 模块包含图形组件和相关类，例如按钮，窗口，状态栏，工具栏，滑块，位图，颜色，字体等。

由于诺基亚作为 QT 工具包的所有者，未能与Riverbank Computing达成协议，PySide 选择将LGPL作为替代许可证。PySide 与 PyQt4 具有高度的 API 兼容性，因此迁移到 PySide 并不困难。

## Python

![pythonlogo](http://zetcode.com/img/gui/pyside/pythonlogo.png) Python 是一种通用的，动态的，面向对象的编程语言。Python 语言的设计目的主要是强调程序员的生产效率和代码可读性。Python 最初是由 Guido van Rossum 开发的，在 1991 进行了第一次发布， 在开发过程中深受 ABC，Haskell，Java，Lisp，Icon 和 Perl 编程语言的影响。同时，Python 是一种高级，通用，多平台的解释语言，并且非常简约，其最明显的特征之一是它不使用分号或括号，而是使用缩进。目前有两个主要的 Python 分支 —— Python 2.x 和 Python 3.x。Python 3.x 打破了与以前版本的 Python 的向后兼容性，主要用来纠正语言的一些设计缺陷，使语言更干净。本教程基于Python 2.x版本，因为当前，大多数代码是用 Python 2.x 版本编写的，将项目迁移到 Python 3.x 还需要一段时间。今天，Python由全世界的一大群志愿者维护，并且已经开源。

Python是那些想要学习编程的人的理想开始。

Python 支持多种编程风格，它不强迫程序员按照特定的一种方式来编程。Python 支持面向对象和过程编程，对函数式编程也有有限的支持。

Python编程语言的官方网站是[python.org](http://python.org/)。

## Python 工具包

为了创建图形用户界面，Python程序员可以有以下这些不错的选项：PySide, PyQt4, Python/Gnome (former PyGTK) 和 wxPython。

本教程是PySide工具包的介绍。







