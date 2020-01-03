🐾 BrowserRouter和HashRouter

🕘 2020.01.03 由 hoanfirst 编辑

For React web projects, react-router-dom provides `<BrowserRouter>` and `<HashRouter>` routers.

The main difference between the two is the way they store the 
URL and communicate with your web server.

- A `<BrowserRouter>` uses regular URL paths. These are generally the best-looking URLs, but they require your server to be configured correctluy. Specifically, your web server needs to server the same page at all URLs that are managed client-side by React Router.\

- A `<HashRouter>` stores the current location in the hash portion of the URL. Since the hash is never sent to the server, this means that no special server configuration is needed.

