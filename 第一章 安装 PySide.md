# 第一章 安装 PySide

标签（空格分隔）： python PySide

---

> 这是一个介绍PySide教程，翻译自 [zetcode](http://zetcode.com/gui/pysidetutorial/)。本教程基于linux进行创建和测试程序，具体的实践环境是 linuxmint18 系统，python 版本是2.7.11+。

本文由藕丝空间提供，如想获取更多知识，请访问[http://www.os373.cn/tag/ZetCode之PySide教程](http://www.os373.cn/tag/ZetCode%E4%B9%8BPySide%E6%95%99%E7%A8%8B)

## 1 建立虚拟环境

虚拟环境是 Python 解释器的一个私有副本,在这个环境中你可以安装私有包,而且不会影响系统中安装的全局 Python 解释器。这样可以在系统的 Python 解释器中避免包的混乱和版本的冲突。 虚拟环境使用第三方实用工具 virtualenv 创建。输入以下命令可以检查系统是否安装了virtualenv:

```bash
$ virtualenv --version
```

如果结果显示错误,你就需要安装这个工具。 安装命令:

```bash
$ sudo apt-get install python-dev python-virtualenv
```

下一步是在项目的目录下创建 Python 虚拟环境。

```bash
$ mkdir gui_code
$ cd gui_code/
$ virtualenv env
Running virtualenv with interpreter /usr/bin/python2
New python executable in /home/ossifrage/gui_code/env/bin/python2
Also creating executable in /home/ossifrage/gui_code/env/bin/python
Installing setuptools, pkg_resources, pip, wheel...done.
```

现在 gui_code 文件夹下有一个名为 env 的子文件夹,它保存了一个全新的虚拟环境,其中有一个私有的 Python 解释器。在使用之前,你需要先将其“激活”。在 linuxmint18 下使用bash shell 命令行:

```bash
$ source env/bin/activate
```

虚拟环境被激活后,其中 Python 解释器的路径就被添加进 PATH 中,但这种改变不是永久性的,它只会影响当前的命令行会话。激活后的命令提示符,加入环境名:

```bash
(env)$
```

如果你想回到局 Python 解释器中,可以在命令提示符下输入deactivate。

```bash
(env)$ deactivate
```

## 2 安装项目

### 2.1 安装构建依赖项

```bash
$ sudo apt-get install build-essential git cmake libqt4-dev libphonon-dev python2.7-dev libxml2-dev libxslt1-dev libqtwebkit-dev
```

### 2.2 安装 PySide

```bash
(env)$ pip install pyside
```

如果不出现错误，恭喜你，安装成功了。

