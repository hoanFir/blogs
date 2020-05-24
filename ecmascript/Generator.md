The Generator object is returned by a `generator function` and it conforms to both the [iterable protocol and the iterator protocol](https://github.com/hoanFir/blogs/blob/master/ecmascript/iteration%20protocols.md).


## generator function/funciton*

The function* declaration defines a generator function, which returns a Generator object.


## Generator

```javascript

//generator function
function* generator() {
  //yield
  yield 1;
  yield 2;
  yield 3;
}

const gen = generator(); // return a Generator object

gen.next()...
gen.return()...
gen.throw()...

```

- 1. Generator.prototype.next()

returns a value yielded by the yield expression

- 2. ...return()

returns the given value and finishes the generator

- 3. ...throw()

throws an error to a generator


## An infinite iterator example

```javascript

function* infinite() {
    let index = 0;

    while (true) {
        yield index++;
    }
}

const generator = infinite(); // "Generator { }"

console.log(generator.next().value); // 0
console.log(generator.next().value); // 1
console.log(generator.next().value); // 2
//...

```
