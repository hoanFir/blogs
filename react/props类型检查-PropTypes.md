
> 注意：自 React v15.5 起，React.PropTypes 已移入另一个包中。请使用 prop-types 库 代替。

随着应用程序不断增长，建议进行整个项目的类型检查，来避免大量错误。

可以在项目引入 Flow 或 TypeScript 等扩展来进行类型检查。但 React 也内置了一些类型检查的功能，比如对组件的 `props` 进行类型检查。

### 一、PropTypes

通过配置

```react

import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};

```

