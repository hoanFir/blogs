🐾 BrowserRouter和HashRouter

🕘 2020.01.03 由 hoanfirst 编辑

### 简介

For React web projects, react-router-dom(to be used in a browser) provides `<BrowserRouter>` and `<HashRouter>` routers.

The main difference between the two is the way they store the 
URL and communicate with your web server.

- A `<BrowserRouter>` uses regular URL paths, which uses HTML5 history API to render the component. These are generally the best-looking URLs, but they require your server to be configured correctluy. Specifically, your web server needs to server the same page at all URLs that are managed client-side by React Router.

- A `<HashRouter>` stores the current location in the hash portion of the URL, and uses the hash in the URL to render the component. Since the hash is never sent to the server, this means that no special server configuration is needed.

### History API for BrowserRouter

As of HTML5, they let us manipulate the contents of the history stack.

1. move backward through history

```
window.history.back()
```

2. move forward

```
window.history.forward()
```

3. move to a specific point in history

```
window.history.go(-1)

window.history.go(1)

window.history.go(-2)

// refreshing the page
window.history.go(0)
window.history.go()

```

4. window.onpopstate

SPA中路由对应组件的实现，就是基于onpopstate来加载不同的组件。

```

window.onpopstate = function(event) {
  alert("location: " + document.location + ", state: " + JSON.stringify(event.state))
}

history.pushState({page: 1}, "title 1", "?page=1")
history.pushState({page: 2}, "title 2", "?page=2")
history.replaceState({page: 3}, "title 3", "?page=3")

history.back() // alerts "location: http://example.com/example.html?page=1, state: {"page":1}"
history.back() // alerts "location: http://example.com/example.html, state: null"
history.go(2)  // alerts "location: http://example.com/example.html?page=3, state: {"page":3}"

```

### Window: hashchange event for HashRouter





