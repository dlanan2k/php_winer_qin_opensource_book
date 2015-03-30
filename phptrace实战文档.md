
# 介绍

phptrace 是一个追踪（trace）PHP执行流程的工具，你如果用过strace的话，则可能很容易想到phptrace到底实现了什么样的功能。 其实，phptrace是类strace的一个实现，不同的是，strace用来追踪系统调用，而phptrace用来追踪PHP函数调用。无论是开发测试 还是线上追查问题，代码执行流程往往会提供许多有用的信息，大大提高了开发人员的工作效率；对于系统函数，我们可以用strace 来观察其调用信息，然而PHP却长久以来缺少这么一个行之有效的工具，因此我们开发了phptrace。

### phptrace 目前包括两部分功能：
+ 打印当前PHP调用栈，
+ 实时追踪PHP调用

# 安装

安装太简单。它的文档在这里：[我去](https://github.com/Qihoo360/phptrace/blob/master/README_ZH.md)

唯一的难点就是要使用git命令或熟悉github的操作。