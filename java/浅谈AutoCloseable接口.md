浅谈AutoCloseable接口
==================

### 一、前言

```
最近在翻看源码时候发现有些类实现了AutoCloseable接口，这个接口很生疏，所以搜了下资料，学习了下，下面做个总结。

```

### 二、AutoCloseable接口由来

```
从AutoCloseable的注释可知它的出现是为了更好的管理资源，准确说是资源的释放，当一个资源类实现了该接口close方法，在使用try-catch-resources语法创建的资源抛出异常后，JVM会自动调用close 方法进行资源释放，当没有抛出异常正常退出try-block时候也会调用close方法。像数据库链接类Connection,io类InputStream或OutputStream都直接或者间接实现了该接口。

2.1 使用AutoCloseable之前资源管理方式

image.png
如上代码创建了两个资源，在try-catch-finally的finally里面进行手动进行资源释放，释放时候还需要进行catch掉异常，这几乎是经典资源使用的方式，那么既然资源管理都是一个套路，那么为何不做到规范里面那？所以AutoCloseable诞生了。

2.2 使用AutoCloseable进行资源管理

image.png
如上图使用jdk1.7新增的try-catch-resources语法在try的()内部创建资源，创建的资源在退出try-block时候会自动调用该资源的close方法。Resource实现了AutoCloseable的close方法：


image.png
运行结果为：


image.png
把read函数里面注释的抛异常代码打开，运行结果为：


image.png
so，从运行结果，总结如下几点

使用try-catch-resources结构无论是否抛出异常在try-block执行完毕后都会调用资源的close方法。
使用try-catch-resources结构创建多个资源，try-block执行完毕后调用的close方法的顺序与创建资源顺序相反
使用try-catch-resources结构，try-block块抛出异常后先执行所有资源（try的（）中声明的）的close方法然后在执行catch里面的代码然后才是finally.
只用在try的()中声明的资源的close方法才会被调用，并且当对象销毁的时候close也不会被调用
使用try-catch-resources结构，无须显示调用资源释放。

```








