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

我们知道，`store`已经通过redux的`Provider`注入到最顶层组件里了。而在子组件中，我们需要通过connect()来获取store提供的状态数据和操作。

```javascript

import React, {Component} from 'react';
import {connect} from 'react-redux';
import {bindActionCreators} from 'redux';

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

- 2)bindActionCreators


### ./actions/LeftMenuActions.js

```javascript
import LeftMenuService from '../services/LeftMenuService';
import MenuActionTypes from '../constants/MenuActionTypes';

let getMenuTree = (dispatch, getState, typeSuccess, typeFaild, params) => {
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
    //因为在app中引入了react-thunk middleware，所以在 action 里面可以使用异步
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

