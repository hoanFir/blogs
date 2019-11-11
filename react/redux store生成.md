🐾 redux store生成

🕘 2019.10.30 由 hoanfirst 编辑

### ./index.js

```javacript

import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';

import configureStore from './store';

import App from './app';

let CONTENT_NODE_WRAP = document.querySelector('#root');

let store = configureStore();

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    CONTENT_NODE_WRAP
);

```

- 1)`react-redux`的`Provider`

makes the Redux store available to any nested components. these nested component that have been wrapped in the `connect()` function.

***Props***:

store - the single redux store in your application.

children - the root of your component hierarchy.

context - provider a context instance.

- 2)App

root-level component.

---

### ./store/index.js

```javascript

import { createStore, applyMiddleware } from 'redux';

import rootReducer from '../reducers';

import thunkMiddleWare from 'redux-thunk';

let middlewares = [
    thunkMiddleWare
];

function configureStore(initialState) {
    let store = applyMiddleware(...middlewares)(createStore)(rootReducer);
    return store;
}

module.exports = configureStore;

```

- 1)createStore

Create a redux store that holds the complete `state tree` of your app. The only way to change the data in the store is to call dispatch() on it. There should only be a single store in your app. To specify how different parts of the state tree respond to actions, you may combine several reducers into a single reducer function by using combineReducers.

`createStore(reducer, \[preloadedState\], enhancer)`

***Props***:

reducer - A function that returns the next state tree, given the current state tree and the action to handle. Such as by using createStore(combineReducers(ReducersMapObject)) 中 combineReducers(reducersMapObject) will return a reducer function that invokes every reducer inside the passed reducersMapObject, and builds a state object with the same shape.

preloadedState - the initial state.

enhancer - the store enhancer. we may to enhance the store with third-party capabilities such as `middleware`, `time travel`, `persistence`, etc. And `middleware` can use with applyMiddleware().

- 2)applyMiddleware

to enhance the store.

Middleware is the suggested way to extend Redux with custom functionality. Middleware lets you wrap the store's `dispatch` method for fun and profit.

the feature of middleware is that it is composable, multiple middleware can be combined together.

`applyMiddleware(...middleware)`

- 3)redux-thunk

the most common use case for middleware is to support `asynchronous actions`. we can `dispatch` async actions in addition to normal actions. **With a plain basic Redux store, you can only do simple synchronous updates by dispatching an action**.

redux-thunk lets the `action creators` invert control by `dispatching functions`, they would receive dispatch as an argument and may call it asynchronously. Such functions are called `thunks`, **a thunk is a function that wraps an expression to delay its evaluation**.

eg:

```javasctipt

// 1. normal action creators

function withdrawMoney(amount) {
    return {
        type: 'WITHDRAW',
        amount,
    }
}
store.dispatch(withdrawMoney(100));


// 2. extend normal action creators' ability with redux-thunk

function withdrawMoneySuccess(userInfo) {
    return {
        type: 'WITHDRAW_SUCCESS',
        userInfo,
    }
}

function withdrawMoneyFailed(id, userInfo) {
    return {
        type: 'WITHDRAW_FAILED',
    }
}

function withdrawMoneyByUsername(id) {
    return function(dispatch) {
        return fetchUsername(id).then(
            (userInfo) => dispatch(withdrawMoneySuccess(userInfo)),
            (error) => dispatch(withdrawMoneyFailed())
        )
    }
}

store.dispatch(withdrawMoneyByUsername(123));

```

tips：在这里只是简单了解一下redux action的基本使用。具体可前往xxx。

---

### ../reducers/index.js

```javascript

import { combineReducers } from 'redux';

import home from './home';
import user from './user';
//import

module.exports = combineReducers({
    home,
    user,
    //reducer
})

```

- 1)combineReducers

Turns an object whose values are different reducer functions, into a single reducer function. It will call every child reducer, and gather their results into a single state object, whose keys correspond to the keys of the passed reducer functions.

我们知道，reducers是用来handle actions的。

When the app is larger, we can split the reducers into separate files and keep them completely independent and managing different data domains.

那么，在没有使用`combineReducers`之前，是如何将这些单独的reducer整合的：

```javascript

const initialState = {
  visibilityFilter: 'SHOW_ALL',
  todos: []
}

function todos(state = initialState.todos, action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case TOGGLE_TODO:
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: !todo.completed
          })
        }
        return todo
      })
    default:
      return state
  }
}

function visibilityFilter(state = initialState.visibilityFilter, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter
    default:
      return state
  }
}

//整合
function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}

```

tips：在这里只是简单了解一下redux reducer的基本使用。具体可前往yyy。

使用`combineReducer`之后：

```javascript

import { combineReducers } from 'redux';

import visibilityFilter from './visibilityFilter/reducers';
import todos from './todos/reducers';

const todoApp = combineReducers({
  visibilityFilter,
  todos,
})

export default todoApp

```

function combineReducers<S>(reducers: ReducersMapObject): Reducer<S>

Turns an object whose values are different reducer functions, into a single reducer function. It will call every child reducer, and gather their results into a single state object, whose keys correspond to the keys of the passed reducer functions.



---

### ../reducers/home.js

```javascript

import createReducer from '../utils/createReducer';

import HomeActionTypes from '../constants/HomeActionTypes';

let initialState = {
    fetchState: 'ING',
    homeData: null,
};

module.exports = createReducer(initialState, {
    [HomeActionTypes.FETCH_STATE](state) {
        return {
            ...state,
            fetchState: 'ING'
        }
    },
    [HomeActionTypes.FETCH_HOME_DATA_SUCCESS](state, action) {
        return {
            ...state,
            homeData: action.data,
            fetchState: 'END'
        }
    },
    [HomeActionTypes.FETCH_HOME_DATA_FAILED](state) {
        return {
            ...state,
            homeData: null,
            fetchState: 'END'
        }
    },
})

```

---

### ../utils/createReducer.js


```javascript

module.exports = function (initialState, actionHandlerMap) {
    return (state = initialState, action) => {
        let handler = null;
        let type = action.type;
        if (type) {
            handler = actionHandlerMap[type];
        }
        if (handler) {
            return handler(state, action);
        }
        return state;
    };
};

```

---

### ../constants/HomeActionTypes.js

```javascript

import createActionTypes from '../utils/createActionTypes';

module.exports = createActionTypes([
    'FETCH_STATE',
    'FETCH_HOME_DATA_SUCCESS', //首页数据获取成功
    'FETCH_HOME_DATA_FAILED', //首页数据获取失败
], 'home_');

```

---

### ../utils/createActionTypes.js

```javascript

module.exports = function (actionTypes, prefix = '') {
    let result = {};
    (actionTypes || []).forEach(actionType => {
        result[actionType] = prefix + actionType;
    });
    return result;
};

```

---

### 总结

到这里，已经为app添加一个store了。
