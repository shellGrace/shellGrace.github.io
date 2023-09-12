### 模块化
模块化的目标是将复杂的前端应用程序拆分为模块，每个模块负责特定的功能。这些模块可以独立开发、测试和维护。模块化解决作用域问题

## 为什么需要 webpack
可以打包你的 JavaScript 应用程序（支持 ESM 和 CommonJS），支持不同的静态资源打包。
1.在浏览器运行 JavaScript：引用脚本来存放每个功能；但加载太多脚本会导致网络加载过慢的问题；或是使用一个包含所有项目代码的 .js 文件，但会导致作用域问题、文件大小、可读性和可维护性方面的问题
2.模块化：CommonJS 允许你在当前文件中加载和使用某个模块。导入需要的每个模块。实现了模块化的功能，但是不被浏览器认可的，因此 需要需要通过工具 Webpack 打包，将其转换为浏览器可执行的代码。
3. IIFE 立即调用函数表达式：IIFE 产出任务执行器，当脚本文件被封装在 IIFE 内部时，可以安全地拼接或安全地组合所有文件，而不必担心作用域冲突

## 常见的 loader
#### babel-loader
通过 .babelrc 配置文件来指定转换规则和babel插件
* 将 ES6 等新版本的 JavaScript 语法转换为 ES5 语法，以确保代码在旧版浏览器中兼容
* 处理 JSX 语法，在 react 中使用

### bebel将ES6转换为ES5
Babel 首先会将输入的 ES6 代码分解为一系列标记（tokens），这些标记是代码的基本词汇单元，包括关键字、标识符、操作符等。使用词法分析产生的标记构建语法树（AST）在 AST 构建完成后，Babel 可以通过一系列插件和转换器来遍历和修改 AST，将 ES6 代码转换为 ES5 代码。包括将 ES6 模块转换为 CommonJS、将箭头函数转换为传统函数等。最后，根据修改后的 AST 生成目标代码

为🐎需要将 ES6 模块转换为 CommonJS？
*服务器环境通常更支持 CommonJS，确保在服务器上运行时没有问题。
*确保与使用CommonJS的库的兼容性

#### css-loader  style-loader 
加载和解析 CSS 文件

#### file-loader
加载文件，如图片、字体等

#### image-loader 
加载和优化图像文件

## 常见的 plugin
* html-webpack-plugin：用于生成 HTML 文件，并将生成的 JavaScript 文件自动插入到 HTML 中;
* clean-webpack-plugin：用于在每次构建之前清理输出目录，以确保输出的文件是最新的;
* eslint-webpack-plugin：用于在构建过程中运行 ESLint 检查
* LimitChunkCountPlugin：用于限制和控制输出的代码块数量
* DefinePlugin：在编译过程中创建全局常量，主要作用是为代码中的条件编译提供静态变量。如区分开发环境和生产环境
