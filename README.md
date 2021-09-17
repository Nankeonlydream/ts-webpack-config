# **TS中配置webpack**



1. **通过npm安装脚手架**

```json
npm install -D webpack webpack-cli
```

2. **配置webpack.config.js**

   ```
   // 引入包(用于拼接路径)
   const path = require('path');
   // 引入html插件
   const HTMLWebpackPlugin = require('html-webpack-plugin');
   //引入clean插件
   const { CleanWebpackPlugin } = require('clean-webpack-plugin');
   
   // webpack中的所有配置信息都应该写在module.exports中
   module.exports = {
     mode: "production",
     // 指定入口文件
     entry: './src/index.ts',
     //指定打包文件所在的目录
     output: {
       // 指定打包文件的目录
       path: path.resolve(__dirname, 'dist'),
       // 打包后文件的名字
       filename: "bundle.js",
   
       // 告诉webpack不适用箭头函数
       environment: {
         arrowFunction: false
       }
     },
   
     //指定webpack打包时要使用的模块
     module: {
       // 指定要加载的规则
       rules: [
         {
           // test指定的是规则生效的文件
           test: /\.ts$/,
           // 要使用的loader
           use: [
             //配置babel
             {
               loader: "babel-loader",
               // 设置babel
               options: {
                 // 设置预定义的环境
                 presets: [
                   [
                     //指定环境插件
                     "@babel/preset-env",
                     // 配置信息
                     {
                       //要兼容的目标浏览器
                       targets: {
                         "chrome": "58",
                         "ie": "11"
                       },
                       //指定corejs的版本
                       "corejs": "3",
                       //使用corejs的方式 "usage" 表示按需加载
                       "useBuiltIns": "usage"
                     }
                   ]
                 ]
               }
             },
             'ts-loader'],
           // 要排除的文件
           exclude: /node-modules/
         },
         // 设置less文件的处理
         {
           test: /\.less$/,
           // 从下往上执行
           use: [
               "style-loader",
               "css-loader",
               // 引入postcss
               {
                 loader: 'postcss-loader',
                 options: {
                   postcssOptions: {
                     plugins: [
                         [
                             "postcss-preset-env",
                           {
                             browser: 'last 2 versions'
                           }
                         ]
                     ]
                   }
                 }
               },
               "less-loader"
           ]
         }
       ]
     },
   
     //配置Webpack插件
     plugins: [
       new CleanWebpackPlugin(),
       new HTMLWebpackPlugin({
         //title: '这是一个自定义的title'
         template: "./src/index.html"
       }),
     ],
   
     // 用来设置引用模块
     resolve: {
       exportsFields: ['.ts', '.js']
     }
   }
   
   
   ```

3. 在package.json文件中加入

```
"build": "webpack"
```

4. 通过命令行执行

```
npm run build
```

5. webpack开发服务器（内置服务器）

```
 npm i -D webpack-dev-server
```

6. 配置tsconfig.json

```
{
  /*
    tsconfig.json是ts编译器的配置文件，ts编译器可以根据它的信息来对代码进行编译
      "include" 用来指定哪些ts文件需要被编译
      路径：/** 表示任意目录
             * 表示任意文件
  */
  "include": [
    "./src/**/*"
  ],
  /*不需要被编译*/
  //  "exclude": [
  //    "./src/hello/*"
  //  ]
  /*
  compilerOptions 指示 TypeScript 编译器如何编译 .ts 文件
  */
  "compilerOptions": {
    // target 为发出的 JavaScript 设置 JavaScript 语言版本并包含兼容的库声明。
    "target": "ES6",
    // module 指定要使用的模块化的规范
    "module": "es2015",
    // 指定一组描述目标运行时环境的捆绑库声明文件
    // "lib": ["ES5","DOM"]
    // outDir 为所有发出的文件指定一个输出文件夹
    "outDir": "./dist",
    // outFile 指定一个将所有输出捆绑到一个 JavaScript 文件中的文件。
    // 设置outFile后，所有的全局作用域中的代码会合并到同一个文件中
//    "outFile": "./dist/app.js"
    // 允许 JavaScript 文件成为您程序的一部分。 使用 `checkJS` 选项从这些文件中获取错误。 是否对js文件进行编译
    "allowJs": true,
    // 是否检查js代码是否符合语法规范，默认是false
    "checkJs": false,
    // 是否移除注释
    "removeComments": true,
    // 不生成编译后的文件
    "noEmit": false,
    // 当有错误时不生成编译后的文件
    "noEmitOnError": true,
    // 启用所有严格的类型检查选项。
    "strict": true,
    // 用来设置编译后的文件是否使用严格模式，默认false
    "alwaysStrict": true,
    // noImplicitAny 为带有隐含“any”类型的表达式和声明启用错误报告。
    "noImplicitAny": true,
    // 当 `this` 的类型为 `any` 时启用错误报告。不允许不明确的类型
    "noImplicitThis": true,
    // 在类型检查时，考虑 `null` 和 `undefined`。
    "strictNullChecks": true
  }
}

```

7. 启动ts热编译

   ```
   tsc -W
   ```

8. package.json

```
{
  "name": "part3",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start": "webpack serve --open chrome.exe"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.15.5",
    "@babel/preset-env": "^7.15.6",
    "babel-loader": "^8.2.2",
    "clean-webpack-plugin": "^4.0.0",
    "core-js": "^3.17.3",
    "html-webpack-plugin": "^5.3.2",
    "ts-loader": "^9.2.5",
    "typescript": "^4.4.3",
    "webpack": "^5.52.1",
    "webpack-cli": "^4.8.0",
    "webpack-dev-server": "^4.2.1"
  }
}

```

9. 启动实时刷新npm
   
```
npm start
```
   
      
