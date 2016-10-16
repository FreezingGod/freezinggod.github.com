---
layout: post
title: Python Virtualenv
---
基于Python的项目在管理的时候，我们经常遇到的一个问题是Python安装和管理第三方软件包十分混乱。第三方软件包的api（甚至包括Python官方软件包）都不能保证向后兼容，基于旧版api开发的软件在新版软件包安装后可能会出现问题，为了不处理由于项目依赖的软件包升级带来的额外工作，可以使用virtualenv对python环境进行管理。

# 什么是pip，跟easy_install有什么关系
简单来讲，pip是一个更加方便的python包管理软件，easy_install可以看作是初级版的包管理软件，而pip则是一个相对来说更高级和方便的管理软件。pip与easy_install的关系类似与ubuntu系统中apt与dpkg的关系。目前科学计算中常用的软件numpy, scipy, scikit-learn, matplotlib, theano乃至tensorflow等都采用pip作为包安装工具。

Virtualenv也是python的一个软件包，提供了环境分离管理的能力，可以使用pip进行安装。一般来讲，使用pip安装的软件默认会被安装到系统级的目录下:
    '/Library/Python/2.7/site-packages/package_name'
使得使用pip安装软件包需要管理权限，同时存在在上文中介绍的api变更导致的向后兼容问题。Virtualenv将python下包管理变得simple and elegent。

# 如何安装使用virtualenv
要安装virtualenv，可以在terminal中执行以下命令：

    sudo pip install virtualenv

即使用pip将virtualenv安装到系统级的包目录内。

Virtualenv有两种使用方法：

## 选项一
不依赖系统级包目录内的软件包建立独立的虚拟环境，这个选项是virtualenv的默认选项。在这种情况下，使用

    sudo pip install package_name

安装到系统级文件夹的软件包在新的virtualenv中不可见亦不可用。如需要使用，需重新安装。
使用这种选项的virtualenv的建立方式十分简单，假设我们的project文件夹在`~/workspace/project`，该project需要安装某一特定版本的软件包，我们可以通过以下方式为该project建立一个独立的虚拟环境，并与其他的环境分离，互不干扰：

    cd ~/workspace/project
    virtualenv env

使用上述命令会在project文件夹下生成一个名为`env`的文件夹，并在该文件夹下copy一份python的可执行文件和site-packages文件夹，用于安装环境内软件包。要使用该环境，可以activate环境：

    source ~/workspace/project/env/bin/activate

上述命令会改变环境变量中的`PATH`和`PYTHONHOME`变量，使得python及pip可执行文件地址指向环境中的可执行文件。一旦activate环境之后，bash和zsh会在prompt中提示当前是一个virtualenv环境，此时通过pip安装的软件包会被安装到虚拟环境的目录下（一般不再需要管理权限），使用python也会优先使用安装到虚拟环境的软件包。
如果需要退出虚拟环境，可以执行：

    deactivate

命令，该命令是一个shell function，会将`PATH`和`PYTHONHOME`环境变量恢复。

## 选项二
依赖和使用安装到系统级文件夹的软件包，即安装到系统级site-packages内的软件包可以复用，所需的新软件包会被安装到virtualenv的安装目录内。要使用该选项，仅需在生成virtualenv的时候添加一个选项：

    virtualenv --system-site-packages env_path

通过上述命令，会在env_path生成一个虚拟环境，环境的开启可退出方法与不依赖系统级包目录的环境一样。

# Best Practice
通常来说一个比较好的做法是在系统级的python软件包目录中仅保留api不会经常发生变动的软件包，为每一个project建立一个独立的virtualenv并单独进行管理。[这篇blog](https://www.dabapps.com/blog/introduction-to-pip-and-virtualenv-python/)推荐的在我看来略显极端的管理方式是：

1. 仅在系统级软件包目录中安装`pip`及`virtualenv`两个软件包。
2. 在项目中按需建立virtualenv并安装依赖软件包。

我认为一些很多软件会依赖的基础包，比如`numpy`，`scipy`等软件包可以安装在系统软件包目录内，针对经常升级并api会变动的软件如`theano`和`tensorflow`等分别建立virtualenv并安装。在使用时按需activate所需环境。

# References:
[A non-magical introduction to pip and virtualenv for python beginners](https://www.dabapps.com/blog/introduction-to-pip-and-virtualenv-python/)
