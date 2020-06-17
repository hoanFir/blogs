## do what

Create a class to store data about a book, and implement a method for displaying the book’s data?


```javascript

//Book(isbn, title, author)
var instanceBook = new Book('0-395-07122-4', 'The Hobbit', 'J. R. R. Tolkien')

instanceBook.display();

```

We will look at examples of the various ways an object can be created and the features available in each.


## 一、fully exposed object

The simplest but provides only public members.

```javascript


var Book = function(isbn, title, author) {
  if(isbn == undefined) throw new Error('Book constructor requires an isbn.');    
  this.isbn = isbn;
  this.title = title || 'No title specified';
  this.author = author || 'No author specified';
}

Book.prototype.display = function() { 
  ...
};

```

But we don’t have any control over what another programmer will assign to the attribute directly. In order to protect the internal data, we can create accessor and mutator methods for each attribute. 

```javascript

var Publication = new Interface('Publication', ['getIsbn', 'setIsbn', 'getTitle', 'setTitle', 'getAuthor', 'setAuthor', 'display']);

var Book = function(isbn, title, author) { // implements Publication 
  this.setIsbn(isbn);
  this.setTitle(title);
  this.setAuthor(author);
}


Book.prototype = {
  checkIsbn: function(isbn) {
    ...
  },
  getIsbn: function() {
    return this.isbn; 
  },
  setIsbn: function(isbn) {
    if(!this.checkIsbn(isbn)) throw new Error('Book: Invalid ISBN.'); 
    this.isbn = isbn;
  },
  getTitle: function() { 
    return this.title;
  },
  setTitle: function(title) {
    this.title = title || 'No title specified'; 
  },
  getAuthor: function() { 
    return this.author;
  },
  setAuthor: function(author) {
    this.author = author || 'No author specified'; 
  },
  display: function() { 
    ...
  }
};

```


## 二、naming convention

Using underscores to denote methods and attributes that are intended to be private

```javascript

var Publication = new Interface('Publication', ['getIsbn', 'setIsbn', 'getTitle', 'setTitle', 'getAuthor', 'setAuthor', 'display']);

var Book = function(isbn, title, author) { // implements Publication 
  this.setIsbn(isbn);
  this.setTitle(title);
  this.setAuthor(author);
}

Book.prototype = {
  _checkIsbn: function(isbn) {
    ...
  },
  getIsbn: function() {
    return this._isbn; 
  },
  setIsbn: function(isbn) {
    if(!this.checkIsbn(isbn)) throw new Error('Book: Invalid ISBN.'); 
    this._isbn = isbn;
  },
  getTitle: function() { 
    return this._title;
  },
  setTitle: function(title) {
    this._title = title || 'No title specified'; 
  },
  getAuthor: function() { 
    return this._author;
  },
  setAuthor: function(author) {
    this._author = author || 'No author specified'; 
  },
  display: function() { 
    ...
  }
};

```

## 三、Scope, Nested Functions, and Closures

Create true private members, which can only be accessed through the use of privileged methods.

First, in JavaScript, only functions have scope; that is to say, a variable declared within a function is not accessible outside of that function. 

So a variable defined within a function is accessible to its nested functions:

```javascript

function foo() {
  var a = 10;
  
  function bar() {
    a *= 2;
  }
  
  bar();
  return a;
}


```

But what if we could execute `bar` outside of `foo`:

```javascript

function foo() { var a = 10;
  function bar() { 
    a *= 2;
    return a;
  }
  return bar; 
}

var baz = foo();
baz(); // returns 20.
baz(); // returns 40.
baz(); // returns 80.

var blat = foo(); // blat is another reference to bar. 
blat(); // returns 20, because a new copy of a is being used.

```

This function is now executed outside of `foo`, and it still has access to `a`. As long as `bar` is defined within `foo`, it has access to all of foo’s variables, even if `foo` is finished executing.

This is an example of a closure. After foo returns, its scope is saved, and only the function that it returns has access to it.

```

var Publication = new Interface('Publication', ['getIsbn', 'setIsbn', 'getTitle', 'setTitle', 'getAuthor', 'setAuthor', 'display']);


var Book = function(newIsbn, newTitle, newAuthor) { // implements Publication

  //private attributes
  var isbn, title, author;
  
  //private method
  function checkIsbn(isbn) {
    ...
  }
  
  //privileged methods
  this.getIsbn = function() {
      return isbn;
    };
  this.setIsbn = function(newIsbn) {
    if(!checkIsbn(newIsbn)) throw new Error('Book: Invalid ISBN.'); 
    isbn = newIsbn;
  };
  this.getTitle = function() { 
    return title;
  };
  this.setTitle = function(newTitle) {
    title = newTitle || 'No title specified';
  };
  this.getAuthor = function() {
    return author;
  };
  this.setAuthor = function(newAuthor) {
    author = newAuthor || 'No author specified'; 
  };

  //constructor code
  this.setIsbn(newIsbn);
  this.setTitle(newTitle); 
  this.setAuthor(newAuthor);
}


// Public, non-privileged methods. 
Book.prototype = {
  display: function() {
    ...
  } 
};


```

In this example, we declared these variables using var, not using this keyword.  That means they will only exist within the Book constructor. We also declare the checkIsbn function in the same way, making it a private method.


