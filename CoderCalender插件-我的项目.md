# CoderCalender

> 插件还在审核中

## 需求分析
> 就想做一个能够看日历，最好能显示每天大事的软件

## 技术选型
> 想用vue做

## 开发环境搭建
1. 使用vue3+vite+Element Plus 搭建
2. 设置vite.config.js

```ts
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'

// https://vitejs.dev/config/
export default defineConfig({
  //++++++++++++++++++++++
  base: './', // !!!!!!! 非常重要，否则打包后无法访问 !!!!!!!
  server: {
    port: 3000,
  },
  //++++++++++++++++++++++
  plugins: [vue(), vueJsx()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```
3. npm run dev 看看成功了吗，成功就继续
4. 在ts.config.js中设置允许JS
```ts
{
  "extends": "@vue/tsconfig/tsconfig.web.json",
  "include": ["env.d.ts", "src/**/*", "src/**/*.vue"],
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    //+++++++++
    "allowJs": true,
    //+++++++++
  },

  "references": [
    {
      "path": "./tsconfig.config.json"
    }
  ]
}

```
5. 按照Utools开发者官网的步骤创建plugin.json,preload.js,logo.png。这三个文件都放在项目根目录的public文件夹下

6. npm run build 打包文件
7. 将dist/下的plugin.json拖入Utools开发者工具
8. ![image-20230121223059397](./images/image-20230121223059397.png)