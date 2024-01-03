<!--
 * @Author: wangzhiyu <w19165802736@163.com>
 * @version: 1.0.0
 * @Date: 2023-12-17 20:30:17
 * @LastEditTime: 2024-01-03 22:25:26
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
