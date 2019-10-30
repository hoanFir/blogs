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
                
                    <SideNav collapsed={collapsed}/>
                    
                    <Layout style={{ marginLeft: collapsed ? 80 : 200 }}>
                        <Header className="layout-header"></Header>
                        <Content className="layout-content">
                            <MyRouter/>
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

```javascript

class MyRouter extends Component {

}

const mapStateToProps = (state) => {
    return {
        fetchState: state.menu.fetchState,
        menus: state.menu.menus,
        menuState: state.menu.menuState,
        auths: state.menu.auths,
        serviceType: state.menu.serviceType
    };
};

const mapDispatchToProps = (dispatch) => {
    return {
        actions: bindActionCreators({
            ...LeftMenuActions,
            ...OptDetailAction
        }, dispatch)
    };
};

module.exports = connect(mapStateToProps, mapDispatchToProps, undefined, {withRef: true, pure: false})(JD_Router);


```
