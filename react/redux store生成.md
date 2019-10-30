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
    </Provider>
    CONTENT_NODE_WRAP
);

```

- 1)`react-redux`的`Provider`

makes the Redux store available to any nested components.

***Props***:

store - the single redux store in your application.

children - the root of your component hierarchy.

context - provider a context instance.

- 2)App

root-level component.


### ./store/index.js

```javascript

import { createStore, applyMiddleware } from 'redux';

import rootReducer from '../reducers';

const thunkMiddleWare = require('redux-thunk').default;

let middlewares = [
    thunkMiddleWare
];

function configureStore(initialState) {
    let store = applyMiddleware(
        ...middlewares
    )(createStore)(rootReducer, initialState);
    
    return store;
}

module.exports = configureStore;

```

- 1)createStore

create a redux store that holds the complete `state tree` of your app.

createStore(reducer, \[preloadedState\], enhancer)

***Props***:

reducer - a reducing function that return the next state tree, given the current state tree and an action to handle.

preloadedState - the initial state.

enhancer - the store enhancer. we may to enhance the store with third-party capabilities such as `middleware`, `time travel`, `persistence`, etc. And `middleware` can use with applyMiddleware().


- 2)applyMiddleware



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

### ../utils/createReducer


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


### ../constants/HomeActionTypes

```javascript

import createActionTypes from '../utils/createActionTypes';

module.exports = createActionTypes([
    'FETCH_STATE',
    'FETCH_HOME_DATA_SUCCESS', //首页数据获取成功
    'FETCH_HOME_DATA_FAILED', //首页数据获取失败
], 'home_');

```

### ../utils/createActionTypes

```javascript

module.exports = function (actionTypes, prefix = '') {
    let result = {};
    (actionTypes || []).forEach(actionType => {
        result[actionType] = prefix + actionType;
    });
    return result;
};

```
