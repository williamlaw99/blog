---
layout: post
title: AndroidStudio调试技巧
category: 工具
tags: Essay
keywords: 
---



### AndroidStudio有两种进入调试的方式

一种是Debug方式：设置好断点，点debug进入调试 ![](http://t1.aixinxi.net/o_1c2qlesvm1noepbmll7h9ncjea.png-w.jpg)

一种是Attach方式：程序先运行了，随时可点Attach debugger to android process进入调试 （很方便）  ![](http://t1.aixinxi.net/o_1c2qlg0oi1kft1cfohlg66rsala.png-w.jpg)



### AndroidStudio Debug常用操作

##### 1. step over : 单步调试（F8）![](http://t1.aixinxi.net/o_1c2qll2gf12u66c8cc713kn1d0ha.png-w.jpg)

跟着程序逻辑一步一步往下走，遇到方法不会进入内部

##### 2. step into: 单步进入（F7）![单步进入](http://t1.aixinxi.net/o_1c2qlhl461ros1kg11sbf19mlrp2a.png-w.jpg)

没遇到方法等同于单步执行，遇到自定义方法会进入，遇到类库的方法不会

##### 3. force step into: 强制单步进入（alt+shift+F7）![强制单步进入](http://t1.aixinxi.net/o_1c2qlu1hdfet10i81fpt1va9ha.png-w.jpg)

遇到任何方法都进入，查看源码时很管用

##### 4. step out: 跳出方法（shift+F8）![](http://t1.aixinxi.net/o_1c2ql1tsog591e1fn7436v1638a.png-w.jpg)

若在方法里则跳出方法；若后面有断点会跳到下一个断点

##### 5. run to Cursor: 跳到下个断点（alt+F9） ![](http://t1.aixinxi.net/o_1c2qlvslpagjvm31nmkt0d1p87a.png-w.jpg)

下个断点（无视是否加了条件）我们见

##### 6. Resume program: 跨断点调试（F9）![](http://t1.aixinxi.net/o_1c2qm5e3t1r3kv553gk1s6a1gtla.png-w.jpg)

如果我们设置了多个断点，需要直接跳转到下一个断点（若加了条件，会跳到满足条件的下一个），那么直接点击它就可以； 

##### 7. 观察变量  ![](http://t1.aixinxi.net/o_1c2qm99ub1nu8oeiahb15j0ut4a.png-w.jpg)

首先，可以在Variables面板看到各变量的值

然后，如果Variables里信息太杂，可以打开Watches面板添加要观察的变量

##### 8. 设置变量值

可以通过快速设置变量的值来加快调试速度，方法是在Variables面板上选中变量右键Set Value

##### 9. 观察表达式 （alt+F8）![](http://t1.aixinxi.net/o_1c2qoj7uq77514p51a221qn0ro9a.png-w.jpg)

可以用变量组合一些表达式，然后观察表达式值

##### 10. Show Execution Point: 显示执行位置 (alt+F10) ![](http://t1.aixinxi.net/o_1c2qmhve61c8o1mq414f6m5s1892a.png-w.jpg)

点击时，光标会跳到当前调试代码执行到的位置

##### 11. View breakpoints: 断点属性设置(ctrl+shift+F8) ![](http://t1.aixinxi.net/o_1c2qmclpovis1akl17fm5peehka.png-w.jpg)

打开一个面板，可以看到所有的断点，以及位置代码,也可以设置一些属性

##### 11. 条件断点

在断点属性设置里：Condition

##### 12. 日志断点

在断点属性设置里：Evaluate and log

##### 13. 临时断点

按住alt，鼠标左键点击添加断点区

##### 14. 失效断点

按住alt，鼠标左键点击已经添加过的有效断点，内部出现绿色圆圈就是失效

