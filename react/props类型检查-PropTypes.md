
> 注意：自 React v15.5 起，React.PropTypes 已移入另一个包中。请使用 prop-types 库 代替。

随着应用程序不断增长，建议进行整个项目的类型检查，来避免大量错误。

可以在项目引入 Flow 或 TypeScript 等扩展来进行类型检查。但 React 也内置了一些类型检查的功能，比如对组件的 `props` 进行类型检查。

### 一、PropTypes.\[types\] and custom validators

To run typechecking on the props for a component, you can assign the spcecail `propTypes` property:

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

`PropTypes` exports a range of **validators** that can be used to make sure the data you receive is valid. When an invalid value is provided for a prop, a warning will be shown in the JavaScript console.

For examples:

```javascript

import PropTypes from 'prop-types';

//1
MyComponent.propTypes = {

  optionalArray: PropTypes.array,
    optionalBool: PropTypes.bool,
      optionalFunc: PropTypes.func,
        optionalNumber: PropTypes.number,
          optionalObject: PropTypes.object,
            optionalString: PropTypes.string,
              optionalSymbol: PropTypes.symbol,
  
  optionalNode: PropTypes.node,
  
  optionalElement: PropTypes.element,
  //with PropTypes.element you can specify that only a single child can be passed to a compoennt as children
  children: PropTypes.element.isRequired


  //delcare that a prop is a react element type(ie. MyComponent)
  optionalElementType: PropTypes.elementType,
  
  //declare that a prop is an instance of a class
  optionalMessage: PropTypes.instanceOf(Message),
  
  //ensure that prop is limited to specific values by treating it as an enum
  optionalEnum: PropTypes.oneof(['news', 'photos']),
  
  //could be one of many types
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceof(Message)
  ]),
  
  //an array of a certain type
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),
  
  //an object with property values of a certain type
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),
  
  //an object taking on a particular shape
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number,
  }),
  
  //an object with warnings on extra properties
  optionalObjectWithStrictShape: PropTypes.exact({
    name: PropTypes.string,
    quantity: PropTypes.number
  }),
}


//2
MyComponent.propTypes = {
  //chain any of the above with 'isRequired'
  requiredFunc: PropTypes.func.isRequired,
  
  //a value of any data type
  requiredAny: PropTypes.any.isRequired,
}


//3
MyComponent.propTypes = {
  //custom validator, which should return an Error object if the validation fails.
  //tips: don't console.warn or throw, because this won't work inside 'oneOfType'
  customProp: function(props, propName, componentName) {
    if(!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop ' + propName + ' supplied to' + componentName + '. Validation failed.'
      )
    }
  },
  
  //custom validator to arrayOf and objectOf
  //the validator will be called for each key in the array or object.
  //the first two arguments of the validator are the array or object it itself
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if(!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop ' + propFullName + ' supplied to' + componentName + '. Validation failed.'
      )      
    }
  })
}


```

