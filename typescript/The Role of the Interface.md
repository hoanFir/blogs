
在 JavaScript 是没有 `interface`, `implements` 这些关键字的，但是 interface 概念在 JavaScript 同样非常重要。

JavaScript 可以通过自身特性来模仿来实现 interface / 接口，比如 class 和 closure。

**参考：《Pro Javascript Design Patterns》**

## 一、Patterns That Rely on the Interface

rely on an interface implementation to work: 

- The factory pattern

Thespecificobjectsthatarecreatedbyafactorycanchange depending on the situation. In order to ensure that the objects created can be used interchangeably, interfaces are used. This means that a factory is guaranteed to pro- duce an object that will implement the needed methods.

- The Composite pattern

Youreallycan’tusethispatternwithoutaninterface.Themost important idea behind the composite is that groups of objects can be treated the same as the constituent objects. This is accomplished by implementing the same interface. Without some form of duck typing or type checking, the composite loses much of its power.

- The decorator pattern

Adecoratorworksbytransparentlywrappinganotherobject.This is accomplished by implementing the exact same interface as the other object; from the outside, the decorator and the object it wraps look identical. We use the Interface class to ensure that any decorator objects created implement the needed methods.

- The command pattern

Allcommandobjectswithinyourcodewillimplementthesame methods (which are usually named execute, run, or undo). By using interfaces, you can create classes that can execute these commands without needing to know anything about them, other than the fact that they implement the correct interface. This allows you to create extremely modular and loosely coupled user interfaces and APIs.


## 二、Encapsulation and Information Hiding

有时候为了不过分依赖某些内部数据，我们会将其封装成 private 数据，通过提供的接口来访问它们。那么，interface 在这样的场景中起什么作用？


It provides a contract that documents the publicly accessible methods. It defines the relationship that two objects can have; either object in this relationship can be replaced as long as the interface is main- tained. 

Most of the time you will find it very helpful to have the available methods documented.

你可以发现，在使用 Typescript 时会经常定义一些 interface，并且能提供 auto-checking.

The ideal software system will define interfaces for all classes. Those classes will provide only the methods defined in their interfaces;

