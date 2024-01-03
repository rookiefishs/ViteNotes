<!--
 * @Author: wangzhiyu <w19165802736@163.com>
 * @version: 1.0.0
 * @Date: 2023-12-17 20:30:17
 * @LastEditTime: 2024-01-03 22:34:38
 * @Descripttion: Vite学习笔记
-->

# Vite 学习笔记

## Vite 功能以及介绍

### 1. [熟悉并使用 vite](https://cn.vitejs.dev/guide/)

1. vite 的组成部分:

   - 一个开发服务器,基于原生 ES 模块提供了丰富的内建功能,例如模块热更新(HMR)
   - 一套构建指令,使用了 rollup 打包代码,并且它是预配置的,可以输出用于生产环境的高度优化过的静态资源

2. 手动搭建 vite 项目(见 01-手动搭建 vite 项目)

   ```js
   // 1. 执行命令行创建模板
   npm create vite@latest 或 yarn create vite

   // 2. 根据需要的框架以及技术栈选择对应的选项


   // 拓展: 使用附加的命令选项直接创建指定的框架与技术栈
   npm create vite@latest my-vue-app -- --template vue // 构建一个vite+vue的项目
   ```

3. 使用社区模板(见 02-使用社区模板)

   ![使用示例](image.png)

   ```js
   // 1. 使用npx引用社区模板
   npx degit user/project#社区模板分支名称 02-使用社区模板

   // 2. 切换目录,初始化依赖包,启动开发服务器
   cd ./02-使用社区模板
   npm i
   npm run dev
   ```

### 2. [Vite 的功能](https://cn.vitejs.dev/guide/features.html)

1. NPM 依赖解析和预构建

   ```js
   // 原生的 ES 模块并不支持下面的裸模块导入,以下代码会在浏览器中抛出错误,但是Vite将会检测到所有被加载的源文件中的此类裸模块导入,并执行处理
   import { data } from 'my-modules';

   // vite针对于裸模块进行的处理
   // 1. 预构建: 预构建可以提高页面的加载速度,并且将CommonJS/UMD转换为ESM格式,预构建这一步骤由esbuild执行,也正是如此,使得Vite的冷启动事件比其余任何基于js的打包器都要快得多
   // 2. 重写url: 针对与裸模块,Vite会在内部对其进行重写,使其成为一个合法的url,可以找到正确的模块路径,例如会将上面的my-modules转换为/node_modules/.vite/deps/my-modules.js?v=f3sf2ebd
   ```

2. 模块热替换

- Vite 准备了一整套原生的 ESM 的 HMR(hot module replace)API,具有 HMR 功能的框架可以利用这些 API 提供实时,准确的更新,不需要加载整个页面以及清楚应用程序的状态,并且 vite 也内置了 HMR 到 Vue 单文件组件(SFC)与 React Fast Refresh 中

- 注意 Vite 的模块热替换不需要手动配置,这些都是 Vite 内置好的功能,只需要根据需求再对配置进行修改即可

- [Vite 的 HMR API 地址](https://cn.vitejs.dev/guide/api-hmr)

3. TypeScript

4. Vue

5. JSX

6. CSS

7. 静态资源处理

8. JSON

9. Glob 导入

10. 动态导入

11. WebAssembly

12. Web Workers

13. 构建优化

## Vite 的配置

### 1. [配置 Vite](https://cn.vitejs.dev/config/)

### 2. [共享配置](https://cn.vitejs.dev/config/shared-options.html)

### 3. [开发服务器选项](https://cn.vitejs.dev/config/server-options.html)

### 4. [构建选项](https://cn.vitejs.dev/config/build-options.html)

### 5. [配置 Vite](https://cn.vitejs.dev/config/)

### 6. [预览选项](https://cn.vitejs.dev/config/preview-options.html)

### 7. [依赖优化选项](https://cn.vitejs.dev/config/dep-optimization-options.html)

### 8. [SSR 选项](https://cn.vitejs.dev/config/ssr-options.html)

### 9. [Worker 选项](https://cn.vitejs.dev/config/worker-options.html)
