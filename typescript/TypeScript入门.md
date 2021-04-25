
TypeScript is a programming language developed by Microsoft.

TypeScript is a **typed superset** of JavaScript, and includes its own compiler.

Being a typed language, TypeScript can catch errors and bugs **at build time**, long before your app goes live.


## 一、引入

To use TypeScript, you need to:

### 1.1 add TypeScript as a dependency to project

### 1.2 configure the TypeScript compiler options

`tsconfig.json`

### 1.3 use the right file extensions

`ts`: the default file extension

`tsx`: a special extension used for files which contain jsx

### 1.4 add difinitions(index.d.ts) for libraries you use

to be able to show errors and hints from other packages, the compiler relies on declaration files.

a declaration file, provides all the type information about a libraray.

this enables us to use js libraries like thoes on npm in our project.

to get declaration file of a library has two main ways:

- 1. Bundled

Bundled（库）它自己包含了自己的 declaration file。这带来的好处就是可以直接使用它。

- 2. DefinitelyTyped(@types/xxx)

DefinitelyTyped 是一个庞大的声明仓库，为没有 declaration file 的库提供类型定义。

比如 React 库没有自己的 declaration file，但可以从 DefinitelyTyped 获取：

```

npm i --save-dev @types/react

```

- 3. 自定义 declaration file

如果 DefinitelyTyped 也没有某个库的 declaration file，这种情况下可以创建一个本地的 declaration file。即在项目根目录创建一个 `declarations.d.ts` 文件，如

```
declare module 'querystring' {
  export function stringify(val: object): string
  export function parse(val: string): object
}

```

## 二、Strictness

a lot of users prefer to have typescript validate as much as it can straight away.

typescript has serveral type-checking strictness flags that can be turned on or off, the `--strict` flag in the CLI, or `"strict": true` in a tsconfig.json.

### 2.1 noImplicitAny

即不要将类型定义为any。

using `any` often defeats the purpose of using typescript. Because the more typed your program is, the more validation and tooling you'll get, meaning you'll run into fewer bugs as you code.

turning on the `noImplicitAny` flag will issue an error on any variables whose type is implicitly inferred as any.


```typescript

let foo: any;

```


### 2.2 strictNullChecks


我们经常会遇到

```typescript

function printName(person) {
  console.log(person.name.toUpperCase());
}
printName(undefined);

Typeerror: undefined is not an object

```

JavaScript中最常见的bug来源之一就是忘记处理空值场景。

by default, values like `null` and `undefined` are assignable to any other type.

The `strictNullChecks` flag makes handling null and undefined more explicit, and spares us from worrying about whether we forgot to handle null and undefined.

当开启之后，上述例子会报错


```typescript

function printName(person: Person) {
  console.log(person.name.toUpperCase());
}
printName(undefined);

// RUNTIME ERROR!  TypeError: undefined is not an object   

```


## 三、the most basic and common types

`string`, `number`, `boolean`, `number[]`, `string[]`, `boolean[]` 

## 四、generics

`T<U>`, `Array<number>`







