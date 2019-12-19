

```

{
    "proxy": [
        [
            "(.*)/open-jimi-web/(.*)/(.*).js",
            "http://127.0.0.1:8088/$3.js",
        ],
        [
            "(.*)/open-jimi-share/(.*)/(.*).js",
            "http://127.0.0.1:8088/$3.js",
        ],
        [
            "(.*)chat.jd.com/(.*).js",
            "http://127.0.0.1:8088/$2.js",
        ],
        [
            "(.*)/__webpack_hmr",
            "http://127.0.0.1:8088/__webpack_hmr",
        ],
        [
            ".min",
            "",
        ],
        [
            "(.*).jd.com/(.*).js",
            "http://127.0.0.1:8088/$2.js",
        ],
        [
            "(.*).sgcc.com.cn/(.*).js",
            "http://127.0.0.1:8088/$2.js",
        ],
        [
        "(.*)/joya.js",
        "//wl.jd.com/joya.js"
        ]
    ],
}

```
