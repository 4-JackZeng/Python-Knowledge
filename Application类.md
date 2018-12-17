### Application类

Application类，Application的主要作用是接收来自httpserver的httprequest，然后根据httprequest中的host和path来寻找匹配的RequestHandler。

`tornado.web`:tornado的基础web框架模块

`tornado.ioloop`:tornado的核心IO循环模块，封装了Linux的epool和BSD的kqueue，是tornado高效的基础

### @staticmethod和@classmethod的作用与区别

一般来说，要使用某个类的方法，需要先实例化一个对象再调用方法。

而**使用@staticmethod或@classmethod，就可以不需要实例化，直接类名.方法名()来调用。**

这有利于组织代码，把某些应该属于某个类的函数给放到那个类里去，同时有利于命名空间的整洁。

既然@staticmethod和@classmethod都可以直接类名.方法名()来调用，那他们有什么区别呢

从它们的使用上来看,

 

- **@staticmethod不需要表示自身对象的self和自身类的cls参数，就跟使用函数一样**。
- **@classmethod也不需要self参数，但第一个参数需要是表示自身类的cls参数。**

如果在@staticmethod中要调用到这个类的一些属性方法，只能直接类名.属性名或类名.方法名。

而@classmethod因为持有cls参数，可以来调用类的属性，类的方法，实例化对象等，避免硬编码。

##### 基本IO模型

网上搜了很多关于同步异步，阻塞非阻塞的说法，理解还是不能很透彻，有必要买书看下。
 参考：[使用异步 I/O 大大提高应用程序的性能](https://link.jianshu.com?t=https://www.ibm.com/developerworks/cn/linux/l-async/)
 [怎样理解阻塞非阻塞与同步异步的区别？](https://link.jianshu.com?t=https://www.zhihu.com/question/19732473)

1. 同步和异步：主要关注消息通信机制（重点在B？）。
   同步：A调用B，B处理直到获得结果，才返回给A。
   异步：A调用B，B直接返回。无需等待结果，B通过状态，通知等来通知A或回调函数来处理。
2. 阻塞和非阻塞：主要关注程序等待（重点在A？）。
   阻塞：A调用B，A被挂起直到B返回结果给A，A继续执行。
   非阻塞：A调用B，A不会被挂起，A可以执行其他操作（但可能A需要轮询检查B是否返回）。
3. 同步阻塞：A调用B，A挂起，B处理直到获得结果，返回给A，A继续执行。
4. 同步非阻塞：A调用B，A继续执行，B处理直到获得结果，处理的同时A轮询检查B是否返回结果。
5. 异步阻塞：异步阻塞 I/O 模型的典型流程 (select)。
6. 异步非阻塞：A调用B，B立即返回，A继续执行，B得到结果后通过状态，通知等通知A或回调函数处理。

