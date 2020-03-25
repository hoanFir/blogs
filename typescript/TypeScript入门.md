
TypeScript is a programming language developed by Microsoft.

TypeScript is a typed superset of JavaScript, and includes its own compiler.

Being a typed language, TypeScript can catch errors and bugs at build time, long before your app goes live.

---

To use TypeScript, you need to:

- **add TypeScript as a dependency to project**

- **configure the TypeScript compiler options**

`tsconfig.json`

- **use the right file extensions**

`ts`: the default file extension

`tsx`: a special extension used for files which contain jsx

- **add difinitions for libraries you use**

to be able to show errors and hints from other packages, the compiler relies on declaration files.

a declaration file, provides all the type information about a libraray. This enables us to use js libraries like thoes on npm in our project.

to get declaration file of a library has two main ways:

1. Bundled

Bundled 是一个库，而且它包含了自己的 declaration file。这样的好处是可以直接使用它。

tips：要知道一个库是否包含类型，看库中是否有 `index.d.ts` 文件。

2. DefinitelyTyped

DefinitelyTyped 是一个庞大的声明仓库，为没有 declaration file 的库提供类型定义。

比如 React 库没有自己的 declaration file，但可以从 DefinitelyTyped 获取：

```
npm i --save-dev @types/react
```

tips：如果 DefinitelyTyped 没有某个库的 declaration file，这种情况
