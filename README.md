# @openeagle/uniapp-using-component-webpack-plugin

`@openeagle/uniapp-using-component-webpack-plugin` 是供 uni-app 使用微信小程序原生组件的 wepack 插件，

## 背景

- 原生组件配置繁琐

    在 uniapp 中要使用第三方 UI 库(vant-weapp，iView-weapp)的时候 ，需要在 src/pages.json 中的 usingComponents 添加对应的组件声明，如：

    ```json
    // src/pages.json
    "usingComponents": {
      "van-button": "/wxcomponents/@vant/weapp/button/index",
    }
    ```

    在开发过程中，我们需要手动一个个配置第三方组件，且在后续维护过程中也要求做好相应的增删 —— 不需要了要记得删掉，但大部分人往往都会忘记处理。

- 手工操作更新依赖

    使用第三方组件，除了在 src/pages.json 还需要在对应的生产目录下建立 wxcomponents，并将第三方的库拷贝至该文件下。这样在升级第三方库版本时需要重新下载依赖，并手动拷贝更新。

- 无用代码占用打包文件

    实际项目可能不会用到全部的第三方组件，但最终打包上传时会将全局配置的所有原生组件都上传上去，导致代码体积增加。

## 优化

使用 webpack 插件自动同步配置原生组件，且在打包上传时通过模板分析，排除掉没用的组件。这样，开发者只需要在项目中引入第三方依赖包。

## 使用

1. 安装依赖

    ```shell
    npm install @openeagle/uniapp-using-component-webpack-plugin --save-dev
    ```

2. 集成配置

    ```js
    // vue.config.js
    const UniappUsingComponentsWebpackPlugin = require('@openeagle/uniapp-using-component-webpack-plugin');

    module.exports = {
      plugins: [
        new UniUsingComponentsWebpackPlugin({
          patterns: [
            {
              prefix: 'van', // 组件的前缀，如 Vant 使用是 van 开头的前缀，iview 使用是 i 开头的前缀，具体可看它们各自的官方文档。
              module: '@vant/weapp', // 第三方 UI 库的包名
            },
            {
              prefix: 'i',
              module: 'iview-weapp',
            },
          ],
        }),
      ],
    }
    ```
