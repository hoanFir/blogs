🐾 使用react hook的useContext

🕘 2019.10.12 由 hoanfirst 编辑


### useContext 特性

接收一个context对象，并返回context的当前值，由上层组件某个`Context.Provider`传递过来的的`value`提供。并当值更新时，该hook就会触发重渲染。注意，该hook其实就是react context的consumer（订阅变化）和contextType（读取值）的代替！！！


- `useContext(MyContext)` 相当于class组件中 `static contextType = MyContext` 或 `<MyContext.Consumer>`。

- `useContext(MyContext)` 使得开发者能够`读取context的值`以及`订阅context的变化`。


### useContext 使用

- `const value = useContext(MyContext)`，接收`React.createContext`返回的context对象，并返回context的当前值。

- 获取的context的当前值由上层组件中距离当前组件最近的`<MyContext.Provider value={this.state}>`中的value决定。


/pages/test/index.tsx

```javascript

import OtherContext, { OtherContextProvider } from '../../context/other-context';

import { TestContextProvider } from '../../context/test-context';

import TestSubComponnet from './subComponent';

const List = () => {
  const {
    mystate: otherstate,
    onDosth: otherOnDosth,
  } = React.useContext(OhterContextProvider);
  
  React.useEffect(()=>{
    onDosth && onDosth();
  }, [1]);
  
  return (
    !otherstate ? <span>loading...</span> :
    <div>
      <Switch>
        <Route exact path="..." component={TestSubComponent} />
        <Route path="..." component={...} />
      </Switch>
    </div>
  )
}

export default () => {
  <OhterContextProvider>
    <TestContextProvider>
        <List />
    </TestContextProvider>
  </OhterContextProvider>
}

```


/contexts/other-context.tsx

```javascript

import * as React from 'react';

//泛型约束
type ContextType = {
  otherstate: String,
  otherOnDosth?: () => PromiseLike<any>,
}

const OtherContext = React.createContext<ContextType>({});
export OtherContext;

export class OtherContextProvider extend React.Component<any, ContextType> {
  componentWillMount() {
    this.setState() {
      otherstate: '',
      otherOnDosth: this.handleDosth,
    }
  }
  
  handleDosth = () => new Promise(async (res) => {
    try {
      const res = await dopost()/doget();
      this.setState({
        otherstate: res.data.state,
      })
      res(res);
    } catch (err) {
      res(null);
      console.log(err.message);
    }
  })
}


```

/pages/test/subComponent.tsx

```javascript


const subComponent = (props) => {
  const { ... } = React.useContext(TestContext);
  ...
  return ( ... )
}

//路由组件可以直接获取路由中的属性，如`history`、`location`、`match`
//而该组件的子组件就不能直接获取，必须通过withRouter修饰路由组件后才能获取到
export default withRouter(subComponent);

```

### 优化

调用了 useContext 的组件总会在 context 值变化时就重新渲染。如果重渲染组件的开销较大，你可以通过使用 memoization 来优化。
