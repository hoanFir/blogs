
## 1. Type Fundamentals

- common JavaScript types

string, number, boolean, array, function, object

```javascript

// adding the colon

let stageName: string = 'a vue';
let roomSize: number = 100;
let isOk: boolean = false;

let list: string[] = ['a', 'b', 'c'];

let foo = (firstName: string, lastName: string): string => {
  return firstName + '' + lastName
}

let person: {
  name: string;
  age: number;
} = {
  name: '',
  age: 123,
}

```

- typescript provides additional types to javascript

**any**, which is essentially disables any type checking.

**tuple**, which is basically a fixed length array with predefined data types.

```javascript

// Example: RGB colors in a array
[number, number, number]

```

**enum**, which allows us to define friendly names to a set of values.


```javascript

enum ArrowKeys {
  Up,
  Down,
  Left,
  Right
 }
 
 // Up === 1, Down === 2, Left === 3, Right === 4

```


## 2. Defining Custom Types

当我们需要定义一个类型，并且限制它的取值。

When we want to define custom types, there are two methods: **type and interface**.

```javascript

//1. type

type buttonType = 'primary' | 'success' | 'danger'
let buttonStyles: buttonType = 'danger'

//2. interface
//it's essentially type, but for objects.

interface Hero = {
  name: string;
  age: number;
}
let person: Hero = {
  name: '',
  age: 123,
}

//3. combine types with interface
type ComicUniverse = '211' | '985'
interface Hero = {
  name: string;
  age: number;
  universe: ComicUniverse;
}
let person: Hero = {
  name: '',
  age: 123,
  universe: '985'
}

```

## 3. Props with Types

## 4. Computed & Methods with Custom Types

## 5. ...
