---
title: "Vue 開發環境請求代理"
date: 2020-05-14T12:24:45+08:00
description: fix CORS policy issue by devServer
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Vue
categories:
- Vue
---

在前後端分離的環境下，在前端需要和公司後台或第三方 API 溝通時，因為請求來自於不同網域、通訊協定或通訊埠時，而遇到跨來源資源共用(CORS)的問題。如果是在開發時遇到這個問題，可以利用開發環境請求代理來解決。
{{< alert theme="danger" >}}
Access to XMLHttpRequest at 'https://www.xxx.com/api' from origin 'http://localhost:8080' has been blocked by CORS policy: No 'Access-Control-Allow-Origin header is present on the requested resource.
{{< /alert >}}

### vue.config.js

Vue 透過配置文件(vue.config.js)來設定開發環境請求代理。
``` js
module.exports = {
    devServer: {
        proxy: {
            target: 'http://localhost:8080'
        }
    }
}
```
這會將任何未知請求代理到 http://localhost:8080。
也可設定多組代理或使用 websocket。
``` js
module.exports = {
    devServer: {
        proxy: {
            '/ws': {
                target: 'wss://localhost',
                ws: true,
                changeOrigin: true
            }，
            '/api': {
                target: 'http://localhost:8080'
            }
        }
    }
}
```

### Reference
[Vue CLI devserver](https://cli.vuejs.org/zh/config/#devserver-proxy)