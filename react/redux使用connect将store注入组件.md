🐾 redux使用connect将store注入组件

🕘 2019.10.30 由 hoanfirst 编辑

### ./app/index.js

```javascript

import React from 'react';
import { HashRouter } from 'react-router-dom';

import Layout from '../components/Layout';
import Header from '../components/Header';
import Content from '../components/Content';

import MyRouter from '../router';

class MyPlatform extends React.Component {
    render() {
        return (
            <HashRouter>
                <Layout className="layout">
                
                    <SideNav collapsed={collapsed}/> //左侧导航栏，通过store.menu.menus来渲染
                    
                    <Layout style={{ marginLeft: collapsed ? 80 : 200 }}>
                        <Header className="layout-header"></Header>
                        <Content className="layout-content">
                            <MyRouter/> //在该组件里获取menus并保存到store.menu.menus里
                        </Content>
                    </Layout>
                    
                </Layout>
            </HashRouter>
        );
    }
}

export default MyPlatform;

```

### ./router/index.js

我们知道，`store`已经通过redux的`Provider`注入到最顶层组件里了。而在子组件中，我们需要通过connect()来获取store提供的状态数据和dispatch操作（当前组件所需要的）。

```javascript

import React, {Component} from 'react';

import { bindActionCreators } from 'redux';
import { connect } from 'react-redux';

import LeftMenuActions from "../actions/LeftMenuActions";

class MyRouter extends Component {
    constructor() {
        super();
    }

    componentDidMount() {
    
        const { actions } = this.props;
        
        actions.getMenuTree();
    }
    
}

const mapStateToProps = (state) => {
    return {
        fetchState: state.menu.fetchState,
        menus: state.menu.menus,
    };
};

const mapDispatchToProps = (dispatch) => {
    return {
        actions: bindActionCreators({
            ...LeftMenuActions,
        }, dispatch)
    };
};

module.exports = connect(mapStateToProps, mapDispatchToProps, undefined, { withRef: true, pure: false })(MyRouter);

```

- 1)connect

The connect() function connects a React component to a Redux store, includes `the pieces of data component needs` and `the functions component can use to dispatch actions to the store`.

connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)

***Props***：

mapStateToProps and mapDispatchToProps - the returns of them are referred to internally as `stateProps` and `dispatchProps`.通过mapStateToProps?: (state, ownProps?) => Object，可以订阅redux store的数据的更新，只要store里面数据发生变化，mapStateToProps()就会被调用，其返回的`stateProps`是一个plain object, which will be merged into the wrapped component's props. 通过mapDispatchToProps?: (dispatch, ownProps?) = > Object，可以将action操作merge into the wrapped component's props. 示例如下：

```javascript

const mapDispatchToProps = dispatch => {
    return {
        //call each function of this object is expected to dispatch an action to the store.
        
        increment: () => dispatch({ type: 'INCREMENT' }),
        decrement: () => dispatch({ type: 'DECREMENT' }),
    }
}

or

import { login, logout } from './actionCreators';
const mapDispatchToProps = dispatch => {
    return {
        login,
        logout,
    }
}

```

connect()返回的是一个函数，该函数会接收component从而返回一个wrapped component with the additional props it injects(注入).


- 2)bindActionCreators

通过使用bindActionCreators，可以

bindActionCreators(actionsCreators, dispatch)


### ./actions/LeftMenuActions.js

tips：在这里只是简单了解一下redux action的基本使用。具体可前往xxx。

```javascript

import LeftMenuService from '../services/LeftMenuService';
import MenuActionTypes from '../constants/MenuActionTypes';

let getMenuTree = (dispatch, getState, typeSuccess, typeFaild, params) => {

    //因为在app中引入了react-thunk middleware，所以在 action 里面可以使用异步

    dispatch({
        type: MenuActionTypes.FETCH_MENU_DATA_START
    });
    
    LeftMenuService.getMenuTree(params).then(function (res) {
        if (res.status === 200 && res.data.state === 'SUCCESS') {
            dispatch({
                type: typeSuccess,
                data: res.data.data
            })
        } else {
            dispatch({
                type: typeFaild,
                data: res.data
            })
        }
    });
}

module.exports = {
    getMenuTree: (params) => {
        return (dispatch, getState) => {
            getMenuTree(dispatch,
                getState,
                MenuActionTypes.FETCH_MENU_DATA_SUCCESS,
                MenuActionTypes.FETCH_MENU_DATA_FAILED,
                params);
        };
    },
}

```

