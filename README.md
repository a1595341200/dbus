# dubs

## 一、介绍

DBUS是一种高级的进程间通信机制。DBUS支持进程间一对一和多对多的对等通信，在多对多的通讯时，需要后台进程的角色去分转消息，当一个进程发消息给另外一个进程时，先发消息到后台进程，再通过后台进程将信息转发到目的进程。DBUS后台进程充当着一个路由器的角色。

DBUS中主要概念为总线，连接到总线的进程可通过总线接收或传递消息，总线收到消息时，根据不同的消息类型进行不同的处理。DBUS中消息分为四类：

1. Methodcall消息：将触发一个函数调用 ；
2. Methodreturn消息：触发函数调用返回的结果；
3. Error消息：触发的函数调用返回一个异常 ；
4. Signal消息：通知，可以看作为事件消息。

## 二、应用场景

根据DBUS消息类型可知，DBUS提供一种高效的进程间通信机制，主要用于进程间函数调用以及进程间信号广播。

1. 函数调用
   
   DBUS可以实现进程间函数调用，进程A发送函数调用的请求（Methodcall消息），经过总线转发至进程B。进程B将应答函数返回值（Method return消息）或者错误消息（Error消息）。
2. 消息广播
   
   进程间消息广播（Signal消息）不需要响应，接收方需要向总线注册感兴趣的消息类型，当总线接收到“Signal消息”类型的消息时，会将消息转发至希望接收的进程。
3. 安装准备:
   
   sudo apt install libdbus-glib-1-dev libdbus-1-dev
4. 代码:
cmake -B build --trace-format=human
cmake --build build

5. 在程序中引用dbus/dbus.h，报错，提示没有该文件。于是在/usr/include下查找，发现dbus的目录名为：/usr/include/dbus-1.0/dbus。 于是在/usr/include下做个软链接:

sudo ln dbus-1.0/dbus/ -s dbus

再次运行程序，原来错误没有了，新的错误出现了，提示dbus-arch-deps.h文件找不到。在系统下搜索该文件位置，然后复制到/usr/include/dbus目录下。

locate dbus-arch-deps.h

查到的结果为

/usr/lib/i386-linux-gnu/dbus-1.0/include/dbus/dbus-arch-deps.h

然后，在/usr/include/dbus目录下，输入:

cp /usr/lib/i386-linux-gnu/dbus-1.0/include/dbus/dbus-arch-deps.h ./
1
再次运行程序，就不报错了。可以正常使用dbus了。
