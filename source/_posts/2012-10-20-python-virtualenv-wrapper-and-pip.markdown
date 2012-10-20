---
layout: post
title: "python virtualenv wrapper and pip"
date: 2012-10-20 14:33
comments: true
categories: Python
---

1.Install
---

Arch 下默认是 `python3`, 而如果强制用软链接改为 `python2` 的话，一些程序会有问题的，之前都是小脚本，所以手动把第一行改为 `#!/usr/bin/env python2` 或者 `#!/usr/bin/python2` 就行，现在要做 GAE 发现手动改了好多还是不行，于是便有了 Python virtualenv 等。

Install virtualenv virtualenvwrapper and pip

    # pacman -S python2-virtualenv
    # pacman -S python2-virtualenvwrapper
    # pacman -S python2-pip

2.Configure
---

Add the following lines to the `~/.bashrc` or `~/.zshrc`

    export WORKON_HOME=~/.virtualenvs
    source /usr/bin/virtualenvwrapper.sh

Then run

    mkdir $WORKON_HOME
    source ~/.zshrc   # or ~/.bashrc
Or manually perform the following operation

    export WORKON_HOME=~/.virtualenvs
    mkdir $WORKON_HOME
    source /usr/bin/virtualenvwrapper.sh

3.Basic Usage
---

Create a virtualenv:

    mkvirtualenv -p python2.7 --no-site-packages hello

We can see:

    Running virtualenv with interpreter /usr/bin/python2.7
    New python executable in hello/bin/python2.7
    Also creating executable in hello/bin/python
    Installing setuptools............................done.
    Installing pip...............done.

We can see them in `~/.virtualenvs/hello`
and find `(hello)` in the Prompt. It means we already activate the virtualenv.
Then we could do something we want, like install some package inside the virtualenv (like, Django):

    (hello)$ pip install django
We could use `deactivate` to leave the virtualenv.

But we also could virtualenv any directory you want, for instance:

    cd ~/gae
    mkdir test
    virtualenv2 -p python2.7 --no-site-packages test

then we could activate the virtualenv.

    cd test
    source bin/activate

then we could do something we want, like install some package inside the virtualenv (like, Django):

    (hello)$ pip install django

Finally, we could similarly use `deactivate` to leave the virtualenv.

4.Virtualenvwrapper
---
We could use Virtualenvwrapper activate a virtualenv:

    $ workon hello

The `hello` is a virtualenv directory in the `$WORKON_HOME`(~/.virtualenvs).
所以我们可以在 `$WORKON_HOME`(~/.virtualenvs) 中做个软链接，然后就能用 `workon` 命令激活

    cd ~/.virtualenvs
    ln -s ~/gae/test .
    workon test

如果频繁切换，容易搞混的话，可以用下面命令来查看自己所处的环境

    which pip
    which python
    which python2
    which python3   # and so on

5.Summary
---
一般来说，一个虚拟环境就放在 `$WORKON_HOME` 就好了，不要和要做的项目工程的源码放在一起，配置好之后就相当于一个稳定的环境了，用的时候切换过去就行了。

See Also
---

[Python VirtualEnv](https://wiki.archlinux.org/index.php/Virtualenv)

[VirtualEnv 和Pip 构建Python的虚拟工作环境](http://www.v2ex.com/t/42760)

[Python开发工具virtualenv的使用 - [Python]](http://blackgu.blogbus.com/logs/170014223.html)
