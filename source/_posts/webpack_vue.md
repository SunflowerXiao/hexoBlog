---
title: webapck+vue
tags:
  -- webapck
categories: 
  - js 
---

1. webpack打包不加随机数

```

module.exports = {
    // webpack配置
    chainWebpack: config => {
        if (process.env.NODE_ENV === 'production') {
            // 清除css，js版本号
            config.output.filename('static/js/[name].js').end();
            config.output.chunkFilename('static/js/[name].js').end();
            // 为生产环境修改配置...
            config.plugin('extract-css').tap(args => [{
                    filename: `static/css/[name].css`,
                    chunkFilename: `static/css/[name].css`
                }])
        }
    },
}

```

