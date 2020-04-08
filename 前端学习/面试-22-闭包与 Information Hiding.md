
Create a class to store data about a book, and implement a method for displaying the book’s data?


```javascript

//Book(isbn, title, author)
var instanceBook = new Book('0-395-07122-4', 'The Hobbit', 'J. R. R. Tolkien')

instanceBook.display();

```

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

## 二、naming convention

Using underscores to denote methods and attributes that are intended to be pri- vate

```javascript

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

## 三、closures

Create true private members, which can only be accessed through the use of privileged methods.



```javascript




```
