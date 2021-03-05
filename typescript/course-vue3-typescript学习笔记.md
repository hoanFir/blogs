
# 一、

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

## 3. Vue data with Custom Types: type assertion & as keyword，类型断言

类型断言，可以用来手动指定一个值的类型，常用于确保传入的是正确的类型。

```javascript

// it will be warning like
let person = null
liveItem.id = ''
liveItem.title = ''


// it's ok to use like
interface liveItem = {
  id: string,
  title: string;
}
let person = {} as liveItem
liveItem.id = ''
liveItem.title = ''

```

## 4. Vue props with Types: generic, 泛型

在软件工程中，不仅要创建一致的定义良好的api，同时也要考虑可重用性，组件不仅能够支持当前扽数据类型，同时也能支持未来的数据类型。

```javascript

funtion createList(item: number): number[] {
  const newList: number[] = [];
  newList.push(item)
  return newList
}
const numberList = createList(1)

// 在上述方法中，方法名更具体应该为 createNumberList。但如果要支持更多类型，怎么定义呢？

funtion createList<CustomType>(item: CustomType): CustomType[] {
  const newList: CustomType[] = [];
  newList.push(item)
  return newList
}
const numberList = createList<number>(1)
const stringList = createList<string>('1')

// 更常用
funtion createList<T>(item: T): T[] {}

```


vue props：

```javascript

import { defineComponent } from 'vue'

expore default defineComponent({
  props: {
    event: {
      type: Object,
      required: true,
    }
  }
})


// with generic...
import { defineComponent, PropType } from 'vue'
import { EventItem } from '../types'

export default defineComponent({
  props: {
    event: {
      //type: EventItem (x)
      //type: Object as EventItem (x)
      type: Object as PropType<EventItem>,
      required: true,
    }
  }
})

```

## 5. Computed Properties & Methods with Custom Types

```javascript

import { defineComponent } from 'vue'
import { EventItem } from '../types'

export default defineComponent({
  data() {
    return {
      events: [] as EventItem[]
    }
  },
  computed: {
    firstEvent(): EventItem {
      return this.events[0]
    }
  },
  methods: {
    addEvent(newEvent: EventItem) {
      this.events.push(newEvent)
    },
    secondEvent(): EventItem {
      return this.events[1]
    },
  }
})

```

# 二、...
