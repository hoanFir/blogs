
#### 6.1 dynamic import()

使用之前：

```javascript

import { add } from './math';
add(12, 13);

```

使用之后：

```javascript

import("./math").then(math => {
  add(12, 13);
})

```

当 Webpack 解析到该语法，会自动进行代码分割。如果使用 create react app，该功能已开箱即用。注意，当使用 babel 时，要确保 babel 能解析动态 import 语法而不是将其进行转换，可以引入 `bable-plugin-syntax-dynamic-import`插件。


#### 6.2 React.lazy or react-loadable

这两个依赖能让开发者想渲染常规组件一样处理 dynamic import。

**决定在哪引入代码分割需要一些技巧，必须确保选择的位置能够均匀分割代码而不会影响用户体验。一个不错的选择是从路由开始**

如 React.lazy

```

const Home = React.lazy(() => {
  import('./Home');
})
const Other = React.lazy(() => {
  import('./OtherComponent');
})

...
<Route exact path="/" component={Home} />
<Route path="/other" component={Other} />
...

```






## 方案一、webpack + es6 import + this.props.children

/routers/index.jsx

```javascript

import Load from '../components/lazy';

let Demo = function() {
  return
    <Load load={() => import('../component/test')}>
      {(Com) => <Com />}
    </Load>
}

```

/components/lazy.jsx

```javascript

import React, { Component } from 'react';

export default class extends Component {

  constructor(props) {
  
    super(props);
    
    this.state={ Comp: null };
    
    this.load(props);
  }
  
  load = (props) => {
  
    props.load().then((Com) => {
      this.setState({ Com: Com.default ? Comdefault : null })
    })
  }
  
  render() {
    if(!this.state.Com) {
      return null;
    } else {
      return this.props.children(this.state.Com);
    }
  }
}

```



## 方案二、webpack + es6 import + higher order component(pure)



/routers/index.jsx

```javascript

import Load from '../components/lazy';

let Demo = Load(() => import('../components/test'));

```

/components/lazy.jsx

```javascript

import React, { Component } from 'react';

export default function(loading) {

  constructor(props) {
  
    super(props);
    
    this.state={ Comp: null };
    
    this.load(props);
  }
  
  load = (props) => {
  
    loading().then((Com) => {
      this.setState({Com: Com.default?Comdefault:null});
    })
  }
  
  render() {
    let Com = this.state.Com;
    return Com ? <Com/> : null;
  }
}

```


## 方案三、webpack + es6 import + async/await


/routers/index.jsx

```javascript

import Load from '../components/lazy';

let Demo = Load(() => import('../components/test'));

```

/components/lazy.jsx

```javascript

import React, { Component } from 'react';

export default class extends Component {

  constructor(props) {
  
    super(props);
    
    this.state={ Comp: null };
  }
  
  async componentDidMount() {
    
    let Com = await loading();
    
    his.setState({ Com: Com.default ? Comdefault : null })
  }
  
  render() {
    let Com = this.state.Com;
    return Com ? <Com/> : null;
  }
}

```


## 方案四、webpack + 基于CommonJS的require.ensure


/routers/index.jsx

```javascript

import Load from '../components/lazy';

let Demo = Load(()=>require('../component/test'));

```

/components/lazy.jsx

```javascript


import React, { Component } from 'react';

export default funtion(loading) {

  rerturn class extends Component {
  
    constructor(props) {
      super(props);
      
      this.state = { Com: null };
    }
    
    componentWillMount() {
    
      new Promise((resolve, reject) => {
      
        require.ensure([], funtion(require) {
          var c = loading().default;
          resolve(c);
        })
        
      }).then(data=>{
        this.setState({Com: data});
      })
    }
    
    render() {
      let Com = this.state.Com;
      return Com ? <Com/> : null; 
    }
  }
}

```


