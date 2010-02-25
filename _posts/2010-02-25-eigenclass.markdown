---
layout: post
title: eigenclass
--- 
{{ page.title }}
================

25 Feb 2010

Ruby中对象的实例变量(instance_variable)是放在对象中，而对象的方法则是放在对象的类里(instance_methods)。我们可以在irb里用下面的命令来验证：

  >> "abc".methods == String.instance_methods

  => true

在ruby里所有都是对象，因此String应该也是对象，下面的命令也应该返回true:

  >> String.methods == String.class.instance_methods

  => false

出乎意料，原因是String的class并不是String.class。class方法并不总是返回对象的真正的类。在Ruby中,对象真正的类是单例类，也可以叫eigenclass,如果这个对象没有eigenclass,它的类才是class方法返回的值。可以用下面的方式获得对象的eigenclass:

  >> eigenclass = class << String
  
  >> self
  
  >> end
  
  => #<Class:String>

这时我们再比较两个对象的方法,看看是否一致：

  >> String.methods == eigenclass.instance_methods

  => true

什么时候对象有eigenclass? 有2种情况,Ruby为你自动生成和自己显式生成。

在声明一个class时，Ruby就会自动为你生成这个class的eigenclass,其他的对象则需要显式生成。通过下面的例子可以看出。

  >> module A

  >> end

  >> class B

  >> end

  >> A.object_id

  => **-610322468**

  >> B.object_id

  => **-610341868**

  >> aeigenclass = class << A

  >> self

  >> end

  => #<Class:A>

  >> aeigenclass.object_id

  => **-610367458**

  >> beigenclass = class << B

  >> self

  >> end

  => #<Class:B>

  >> beigenclass.object_id
  
  => **-610341878**

  >> class B

  >> include A

  >> end

  => B

  >> aBeigenclass.object_id

  => **-610341878**  <==在class中include module不会生成新的eigenclass

在这里通过比较object_id的大小可以看出对象创建的顺序。B是class，它的object_id后面就是B的eigenclass的object_id。而A是module，它的eigenclass在显式赋值时才被创建。


