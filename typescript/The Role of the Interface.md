
在 JavaScript 是没有 `interface`, `implements` 这些关键字的，但是 interface 概念在 JavaScript 同样非常重要。

JavaScript 可以通过自身特性来模仿 interface。

**参考：《Pro Javascript Design Patterns》**

## Emulating an Interface in js

1. Describing Interfaces with Comments

```javascript

/*
interface Composite {
    function add(child);
    function remove(child);
    function getChild(index);
}
interface FormItem {
    function save();
}
*/

// implements Composite, FormItem ...
var CompositeForm = function(id, method, action) {
  //...
};

// Implement the Composite interface.
CompositeForm.prototype.add = function(child) { ...
};
CompositeForm.prototype.remove = function(child) {
...
};
CompositeForm.prototype.getChild = function(index) {
...
};

// Implement the FormItem interface.
CompositeForm.prototype.save = function() { ...
};

```

2. Emulating Interfaces with Attribute Checking

```javascript

/*
interface Composite {
    function add(child);
    function remove(child);
    function getChild(index);
}

interface FormItem {
    function save();
}
*/

var CompositeForm = function(id, method, action) {  

    this.implementsInterfaces = ['Composite', 'FormItem'];
    //...
};

...

function addForm(formInstance) {
    if(!implements(formInstance, 'Composite', 'FormItem')) {
throw new Error("Object does not implement a required interface.");     }
    ...
}

// The implements function, which checks to see if an object declares that it 
// implements the required interfaces.
function implements(object) {
    for(var i = 1; i < arguments.length; i++) { 
        // Looping through all arguments
        // after the first one.
        
        var interfaceName = arguments[i];
        var interfaceFound = false;
        for(var j = 0; j < object.implementsInterfaces.length; j++) {
            if(object.implementsInterfaces[j] == interfaceName) {                   
                interfaceFound = true;
                break;
            } 
        }

        if(!interfaceFound) {
            return false; // An interface was not found.
        } 
    }
    
    return true; 
    // All interfaces were found. 

}

```

3. Emulating Interfaces with Duck Type

```javascript

// Interfaces.
var Composite = new Interface('Composite', ['add', 'remove', 'getChild']); 
var FormItem = new Interface('FormItem', ['save']);

// CompositeForm class
var CompositeForm = function(id, method, action) { 
    //...
};

...
function addForm(formInstance) {
    ensureImplements(formInstance, Composite, FormItem);
    // This function will throw an error if a required method is not implemented. ...
}

```


## Patterns that Rely on the Interface

rely on an interface implementation to work: 

- The factory pattern

Thespecificobjectsthatarecreatedbyafactorycanchange depending on the situation. In order to ensure that the objects created can be used interchangeably, interfaces are used. This means that a factory is guaranteed to pro- duce an object that will implement the needed methods.

- The Composite pattern

Youreallycan’tusethispatternwithoutaninterface.Themost important idea behind the composite is that groups of objects can be treated the same as the constituent objects. This is accomplished by implementing the same interface. Without some form of duck typing or type checking, the composite loses much of its power.

- The decorator pattern

Adecoratorworksbytransparentlywrappinganotherobject.This is accomplished by implementing the exact same interface as the other object; from the outside, the decorator and the object it wraps look identical. We use the Interface class to ensure that any decorator objects created implement the needed methods.

- The command pattern

Allcommandobjectswithinyourcodewillimplementthesame methods (which are usually named execute, run, or undo). By using interfaces, you can create classes that can execute these commands without needing to know anything about them, other than the fact that they implement the correct interface. This allows you to create extremely modular and loosely coupled user interfaces and APIs.


## Encapsulation and Information Hiding

有时候为了不过分依赖某些内部数据，我们会将其封装成 private 数据，通过提供的接口来访问它们。

那么，interface 在这样的场景中起什么作用？

It provides a contract that documents the publicly accessible methods. It defines the relationship that two objects can have; either object in this relationship can be replaced as long as the interface is maintained. Most of the time you will find it very helpful to have the available methods documented.

你可以发现，在使用 Typescript 时会经常定义一些 interface，并且能提供 auto-checking.

The ideal software system will define interfaces for all classes. Those classes will provide only the methods defined in their interfaces;

