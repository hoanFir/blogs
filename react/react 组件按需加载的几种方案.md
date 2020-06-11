

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


## 方案四、webpack + common.js require.ensure


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


