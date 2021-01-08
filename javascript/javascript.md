##  Js

### 1.js的属性

```
let proxyObj = {};
const CompressionPlugin = require("compression-webpack-plugin");
proxyObj['/ws'] = {
    ws: true,
    target: "ws://localhost:8081"
};
proxyObj['/'] = {
    ws: false,
    target: 'http://localhost:8081',
    changeOrigin: true,
    pathRewrite: {
        '^/': ''
    }
}
```


