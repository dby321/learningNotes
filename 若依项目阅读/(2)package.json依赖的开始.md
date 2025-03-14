# Package.json依赖的开始

## 1. Husky 和 lint-staged 配置解析

#### 1. **Husky 配置**

```json
"husky": { 
    "hooks": 
    { 
        "pre-commit": "lint-staged"
    }
}
```

- **作用**：Husky 是一个用于在 Git 提交过程中执行脚本的工具，通过定义 Git hooks 来自动化某些任务。
- **具体功能**：
  - `pre-commit`：在每次执行 `git commit` 前触发。
  - 触发时会运行 `lint-staged`，对即将提交的代码进行格式化和检查。

```js
"lint-staged": {
    "src/**/*.{js,vue}": [
      "eslint --fix",
      "git add"
    ]
  },
```

- **作用**：`lint-staged` 是一个工具，用于对暂存区（staged）中的文件运行特定命令，确保提交的代码符合规范。
- **具体功能**：
  - `"src/**/*.{js,vue}"`：匹配 `src` 目录下所有 `.js` 和 `.vue` 文件。
  - `[ "eslint --fix", "git add" ]`：
    1. 使用 ESLint 对匹配到的文件进行检查并自动修复可修复的问题。
    2. 自动将修复后的文件重新添加到暂存区（`git add`），确保提交的

## 2. script的作用

```js
  "scripts": {
    "dev": "vue-cli-service serve",
    "build:prod": "vue-cli-service build",
    "build:stage": "vue-cli-service build --mode staging",
    "preview": "node build/index.js --preview",
    "lint": "eslint --ext .js,.vue src"
  },
```



该 `scripts` 配置提供了常用的开发、构建和代码检查命令，便于开发者快速操作：
- `dev`：启动开发服务器。
- `build:prod` 和 `build:stage`：分别用于生产环境和预发布环境的构建。
- `preview`：预览构建后的静态文件,用作上线前帮助其他成员或非技术人员（如产品经理、设计师）快速查看最新的构建结果
- `lint`：检查代码规范，提升代码质量，使用 ESLint 检查 `src` 目录下的 `.js` 和 `.vue` 文件

## 3. browsersList

- **> 1%**  
  表示覆盖全球至少 1% 用户的浏览器版本。这意味着只会支持那些在全球范围内使用率超过 1% 的浏览器版本。

- **last 2 versions**  
  表示支持每个浏览器的最后两个版本。例如，如果 Chrome 的最新版本是 110，那么会支持 Chrome 110 和 109。

## 4. dependencies

1. **`@riophae/vue-treeselect: "0.4.0"`**  
   - 一个基于 Vue 的树形下拉选择组件，适用于多级分类或层级结构的选择场景。

2. **`axios: "0.24.0"`**  
   - 用于发起 HTTP 请求的库，支持浏览器和 Node.js 环境，常用于与后端 API 进行交互。

3. **`clipboard: "2.0.8"`**  
   - 提供复制和粘贴功能的库，可以轻松实现将内容复制到剪贴板的需求。

4. **`core-js: "3.25.3"`**  
   - 一个 polyfill 库，用于为旧版浏览器提供现代 JavaScript 特性（如 Promise、Array 方法等）的支持。

5. **`echarts: "5.4.0"`**  
   - 百度开源的数据可视化图表库，支持丰富的图表类型，适用于数据展示和分析场景。

6. **`element-ui: "2.15.13"`**  
   - 基于 Vue 的桌面端 UI 组件库，提供了大量开箱即用的组件（如按钮、表格、对话框等），适合快速开发后台管理系统。

7. **`file-saver: "2.0.5"`**  
   - 用于保存文件的库，支持将 Blob 对象保存为本地文件，常用于导出文件功能。

8. **`fuse.js: "6.4.3"`**  
   - 一个轻量级的模糊搜索库，适用于需要实现搜索功能的场景。

9. **`highlight.js: "9.18.5"`**  
   - 代码高亮库，用于在网页中显示带有语法高亮的代码块。

10. **`js-beautify: "1.13.0"`**  
    - 用于格式化代码的库，支持 HTML、CSS 和 JavaScript 的美化。

11. **`js-cookie: "3.0.1"`**  
    - 一个简单易用的 Cookie 操作库，用于在客户端存储少量数据。

12. **`jsencrypt: "3.0.0-rc.1"`**  
    - 用于前端加密的库，支持 RSA 加密算法，常用于用户登录时对密码进行加密传输。

13. **`less-loader: "^5.0.0"`**  
    - Webpack 的 Less 文件加载器，用于将 Less 文件编译为 CSS。

14. **`nprogress: "0.2.0"`**  
    - 一个小型的进度条库，常用于页面加载或 AJAX 请求时显示进度。

15. **`quill: "1.3.7"`**  
    - 一个功能强大的富文本编辑器，支持多种自定义样式和插件。

16. **`screenfull: "5.0.2"`**  
    - 用于控制全屏模式的库，支持检测和切换全屏状态。

17. **`sortablejs: "1.10.2"`**  
    - 一个拖拽排序库，适用于需要实现列表拖拽排序功能的场景。

18. **`store: "^2.0.12"`**  
    - 一个简单的前端存储库，支持 localStorage 和 sessionStorage 的统一操作。

19. **`vant: "^2.13.0"`**  
    - 蚂蚁金服出品的移动端 Vue 组件库，适用于移动端界面开发。

20. **`vue: "2.6.12"`**  
    - Vue.js 框架的核心库，用于构建用户界面。

21. **`vue-count-to: "1.0.13"`**  
    - 一个数字渐变动画的 Vue 插件，适用于数据展示时的动态效果。

22. **`vue-cropper: "0.5.5"`**  
    - 一个图片裁剪组件，支持上传图片后的裁剪操作。

23. **`vue-meta: "2.4.0"`**  
    - 用于管理 Vue 应用中的 meta 标签和其他头部信息的插件。

24. **`vue-router: "3.4.9"`**  
    - Vue.js 的官方路由管理库，用于实现单页应用的页面导航。

25. **`vuedraggable: "2.24.3"`**  
    - 一个基于 Sortable.js 的 Vue 组件，用于实现拖拽排序功能。

26. **`vuex: "3.6.0"`**  
    - Vue.js 的官方状态管理库，用于集中管理应用的状态。

## 5. devDependencies

1. **`@vue/cli-plugin-babel: "4.4.6"`**  
   - Vue CLI 的 Babel 插件，用于将现代 JavaScript 语法转换为兼容性更好的代码。

2. **`@vue/cli-plugin-eslint: "4.4.6"`**  
   - Vue CLI 的 ESLint 插件，用于代码风格检查和自动修复。

3. **`@vue/cli-service: "4.4.6"`**  
   - Vue CLI 的核心服务，提供开发服务器、构建工具等功能。

4. **`babel-eslint: "10.1.0"`**  
   - 允许 ESLint 解析由 Babel 转换后的代码，支持更多语法特性。

5. **`babel-plugin-dynamic-import-node: "2.3.3"`**  
   - 一个 Babel 插件，用于将动态导入（`import()`）转换为 Node.js 的 `require`，适用于服务端渲染。

6. **`chalk: "4.1.0"`**  
   - 用于在终端输出带颜色的文字，常用于增强命令行工具的可读性。

7. **`compression-webpack-plugin: "5.0.2"`**  
   - Webpack 插件，用于压缩生成的静态资源文件（如 JS 和 CSS），提升加载性能。

8. **`connect: "3.6.6"`**  
   - 一个轻量级的 HTTP 服务器框架，常用于开发环境中的中间件处理。

9. **`eslint: "7.15.0"`**  
   - 一个代码质量检查工具，用于检测和修复 JavaScript 代码中的问题。

10. **`eslint-plugin-vue: "7.2.0"`**  
    - ESLint 的 Vue 插件，支持对 `.vue` 文件进行代码风格检查。

11. **`less: "^2.3.1"`**  
    - Less 预处理器，用于编写更简洁和模块化的 CSS。

12. **`lint-staged: "10.5.3"`**  
    - 在提交代码时对暂存区的文件运行指定的 lint 工具，确保代码符合规范。

13. **`runjs: "4.4.2"`**  
    - 一个用于快速执行 JavaScript 脚本的工具，简化开发流程。

14. **`sass: "1.32.13"`**  
    - Sass 预处理器，用于编写更强大的 CSS。

15. **`sass-loader: "10.1.1"`**  
    - Webpack 的 Sass 加载器，用于将 Sass 文件编译为 CSS。

16. **`script-ext-html-webpack-plugin: "2.1.5"`**  
    - Webpack 插件，用于扩展 HTML 中的 `<script>` 标签功能，例如内联运行时代码。

17. **`svg-sprite-loader: "5.1.1"`**  
    - Webpack 的 SVG Sprite 加载器，用于将多个 SVG 图标合并为一个雪碧图，减少 HTTP 请求。

18. **`vue-template-compiler: "2.6.12"`**  
    - Vue 模板编译器，用于将 `.vue` 文件编译为可渲染的 JavaScript。