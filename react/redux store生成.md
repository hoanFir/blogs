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



- 2)applyMiddleware
