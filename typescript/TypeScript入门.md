
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

## 二、


