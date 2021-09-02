# vite

> 下一代前端开发与构建工具
>
> 新型开发工具 显著提升速度 发音/vit/
>
> - 构造
>   - 开发服务器 HMR速度快
>   - 一套构建命令 使用rollup打开

## 安装

- 全局安装

  ```shell
  npm i vite -g # 全局安装
  npm i vite -D # 局部安装
  ```

- 安装项目

  - NPM	

    ```shell
    npm init @vitejs/app
    ```

  - yarn

    ```shell
    yarn create @vitejs/app
    ```

## 开启服务器

- 启动静态资源服务器

  ```shell
  vite
  ```

- 对vue文件的支持

  - **编译.vue**

    ```shell
    @vue/compiler-sfc -D
    ```

  - vue3单文件组件支持

    ```shell
    @vitejs/plugin-vue
    ```

  - vue3 jsx支持

    ```shell
    @vitejs/plugin-vue-jsx
    ```

  - vue2支持

    ```shell
    underfin/vite-plugin-vue2
    ```

  > 添加vite.config.js配置文件
  >
  > ```js
  > const vue = require("@vitejs/plugin-vue")
  > 
  > module.exports = {
  >     plugins:[
  >         vue()
  >     ]
  > }
  > ```

  

## 打包

- 命令

  ```shell
  vite build
  ```

- 预览命令(打包后 )

  ```shell
  vite preview
  ```

- ### ES build的特点

  - 超快构建速度 不需要缓存
  - 支持`ES6`和`commonjs`
  - 支持`ES6`的`tree shaking`
  - 支持`go`,`js`的api
  - 支持`ts`,`jsx`
  - 支持`SourceMap`
  - 支持代码压缩
  - 支持扩展其他插件

## vite脚手架

- 安装

  ```shell
  npm i @vitejs/create-app -g
  ```

-  初始化

  ```shell
  create-app demo
  ```

- 选择vue

- 下载依赖

  ```shell
  npm i
  ```

- 项目结构

  > 类似于vue-cli项目结构





