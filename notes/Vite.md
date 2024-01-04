<!--
 * @Author: wangzhiyu <w19165802736@163.com>
 * @version: 1.0.0
 * @Date: 2023-12-17 20:30:17
 * @LastEditTime: 2024-01-04 21:51:35
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

   - 仅执行转译:

     - 简介: Vite 只会执行对.ts 文件的转译工作,并不会执行任何类型检查,因为类型检查已经被开发使用的 IDE 编辑器在构建过程中处理过了
     - 原因: Vite 之所以不将类型检查作为转换过程中的一部分,是因为这两项工作在本质上是不同的,转译可以在每个文件基础上进行,与 Vite 的编译模式完全吻合,但是类型检查需要了解整个模块图,将类型检查塞进 Vite 的转换通道,将会不可避免的影响 Vite 的速度优势
     - 补充: 如果在开发时需要更多的 IDE 提示,建议在一个单独的进程中运行 tsc -noEmit -watch,或者希望在浏览器中直接看到错误,可以使用 vite-plugin-checker 插件

   - TypeScript 编译器选项

     - tsconfig.json 中的 compilerOptions 下的一些配置项需要特别注意

       ```js
       // isolatedModules(https://www.typescriptlang.org/tsconfig#isolatedModules) 应该设置为true
       // 1. 这是因为esbuild只会执行没有类型信息的转译,它并不支持某些特性,例如const enum和隐式类型导入
       // 2. 所以必须tsconfig.json中的compilerOptions下设置"isolatedModules":true,这样做,ts就会警告开发者不要使用隔离(isolated)转译的功能
       // 3. 补充: 一些库不能很好的与"isolateMoudles":true,这个问题可以在上游仓库修复好之前 暂时的使用"skipLibCheck":true来缓解这个错误

       // useDefineForClassFields(https://www.typescriptlang.org/tsconfig#useDefineForClassFields)
       // 1. 从Vite2.5.0开始,如果ts的target是ESNext或ES200,此选项默认为true,这与tsc 4.3.2以及以后版本的行为一致,这也是标准的ECMAScript的运行时行为
       // 2. 如果设置了其他ts目标,则本项会默认为false

       // target(https://www.typescriptlang.org/tsconfig#target) Vite不会默认转译TSD,而是使用esbuild的默认行为
       // 1. 此esbuild.target选项可以用来代替上述行为,默认值为esnext,以进行最小的转译,在构建中,build.target选项优先级更好,如果需要也可以设置
       // 2. 可能影响构建见过的其他编译器选项: estends importsNotUseAsValues preserveValueImports jsx jsxFactory jsxImportSoure experimentalDecorators alwaysStrict skipLibCheck  Vite启动模板默认情况下会设置"skipLibCheck":"true",以避免对依赖项进行检查,因为它们可能支持特定版本和配置的TS
       ```

   - 客户端类型

     - Vite 默认的类型定义式写给它的 NodejsAPI 的,要将其补充到一个 Vite 应用的客户端代码环境中,需要添加 d.ts 声明文件

       ```js
       /// <reference types="vite/client"/>
       // 或者,可以将vite/client 添加到tsconfig.json中的compilerOptions.types下:
       {
        "compilerOptions":{
          "types":["vite/client"]
        }
       }
       ```

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
