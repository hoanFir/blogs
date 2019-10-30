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

Props:

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

Props:

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

```




