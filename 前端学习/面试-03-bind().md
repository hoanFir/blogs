
At some point, you might be inclined to set this to a variable that you can reference **when you change context**.

Many people opt for `self`, `_this` or sometimes `context` as a variable name. They're all usable and nothing is wrong with doing that, but there is a better, dedicated way.

### 一、old

here is sample code in which one could be forgiven for caching the context to a variable:

```javascript

var myObj = {

  specialFunction: function() {},
  
  getAsyncData: function(cb) { cb(); },
  
  render: function() {
    var that = this;
    this.getAsyncData(function() {
      that.specialFunction();
    })
  }

}

myObj.render();

```

We need to keep the context of the `myObj` object referenced for when the callback function is called. Calling `that.specialFcuntion()` enables us to maintain that context and conrrectly execute our function.


### 二、Function.prototype.bind()


It is neatened somewhat by using `Function.prototype.bind()`.

```javascript

```javascript

var myObj = {

  specialFunction: function() {},
  
  getAsyncData: function(cb) { cb(); },
  
  render: function() {
    this.getAsyncData(function() {
      this.specialFunction();
    }.bind(this))
  }

}

myObj.render();

```

### 三、What bind() does internally

here is a simple example to see what `Function.prototype.bind()` might look like and what its doing internally:

```javascript

Function.prototype.bind = function(scope) {

  var fn = this;
  
  return function() {
    return fn.apply(scope); //apply会立即执行
  }

}

```
