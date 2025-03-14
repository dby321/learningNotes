# babel.config.js



```js
module.exports = {
  presets: [
    // https://github.com/vuejs/vue-cli/tree/master/packages/@vue/babel-preset-app
    '@vue/cli-plugin-babel/preset'
  ],
  'env': {
    'development': {
      // babel-plugin-dynamic-import-node plugin only does one thing by converting all import() to require().
      // This plugin can significantly increase the speed of hot updates, when you have a large number of pages.
      'plugins': ['dynamic-import-node']
    }
  }
}
```

## 1. **整体结构**

`babel.config.js` 是 Babel 的配置文件，用于定义如何将现代 JavaScript 代码转换为兼容性更好的代码。

## 2. **`presets` 字段**

1. **作用**：  
   `presets` 定义了 Babel 的预设（preset），预设是一组插件的集合，用于快速配置 Babel。
2. **具体功能**：  
  - `@vue/cli-plugin-babel/preset` 是 Vue CLI 提供的官方 Babel 预设，包含以下功能：
    1. 支持 ES6+ 语法（如箭头函数、解构赋值等）。
    2. 支持动态导入（`import()`）。
    3. 自动处理 Vue 单文件组件（`.vue` 文件）中的 `<script>` 部分。

3. **参考链接**：  
  [Vue CLI Babel Preset](https://github.com/vuejs/vue-cli/tree/master/packages/@vue/babel-preset-app)

## 3. **`env` 字段**

- **作用**：  
  `env` 字段允许根据不同的环境（如开发环境或生产环境）应用不同的 Babel 插件。

- **具体功能**：  
  - 在 `development` 环境中，启用了 `dynamic-import-node` 插件。
  - **`dynamic-import-node` 插件的作用**：
    - 将所有的动态导入（`import()`）转换为同步的 `require()`。
    - 这样可以显著提升热更新（HMR, Hot Module Replacement）的速度，尤其是在项目中有大量页面时。

- **适用场景**：  
  - 开发环境中需要频繁修改代码并查看效果，使用 `dynamic-import-node` 可以减少动态导入带来的性能开销，加快开发体验。