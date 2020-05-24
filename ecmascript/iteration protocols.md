
As a couple of additions to ECMAScript 2015, Iteration protocols aren't new built-ins or syntax, but protocols.

There are two protocols: **iterable protocol** and **iterator protocol**.

## iterable protocol

the iterable protocol allows Javascript **objects** to define or customize their iteration behavior.

In order to be iterable, an object must implement the `@@iterator` method, meaning that the object must have a property with `@@iterator` key which is available via constant `Symbol.iterator`.

Whenever an object needs to be iterated (such as at the beginning of a for...of loop), its `@@iterator` method is called with no arguments, and the returned iterator is used to obtain the values to be iterated.

Note that when this zero-argument function is called, it is invoked as a method on the iterable object. Therefore inside of the function, the `this` keyword can be used to access the properties of the iterable object, to decide what to provide during the iteration.

## iterator protocol
