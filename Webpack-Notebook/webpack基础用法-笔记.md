1. ·构建机制

   模块打包器，  会将所有文件当作模块。



2. entry

   指定打包入口

   单入口：

   ```
   module.exports = {
   	entry: './src/index.js'
   }
   ```

   多入口

   ```
   module.exports = {
   	entry: {
   		index: './src/index.js'
   		about: './src/about.js'
   	}
   }
   ```

   

3. Output

   告诉webpack 如何将编译后的文件输出到磁盘，   磁盘目录以及文件名称

   ```javascript
   output: {
   	filename: [name].js    // 'bundle.js',
   	path: __dirname + '/dist'
   }
   ```

   filename: 用`[]`占位符区分**多入口**打包的结果, 如果是单入口的就不需要这么写



4. Loader

   Webpack 开箱即用只支持 JS 和 JSON 两种文件类型。 通过Loaders去支持其他类型，并且把它们转化成有效的模块，并且可以添加到依赖图。

   本身是一个函数， 接受 **源文件作为参数**， **返回转换的结果**。

   

   常用Loaders

   | 名称          | 描述                       |
   | ------------- | -------------------------- |
   | Babel-loader  | 转换ES6、ES7等JS新特性语法 |
   | Css-loader    | 支持css文件的加载与解析    |
   | Ts-loader     | 将Ts转换成JS               |
   | File-loader   | 进行图片、字体等的打包     |
   | Raw-loader    | 将文件以字符串的形式导入   |
   | Thread-loader | 多进程打包JS和css          |
   | Less-loader   | 将less文件转换成css        |
   |               |                            |

   ```javascript
   module.exports = {
     output: {
       filename: ''
     }
     module: {
     	rules: [
     		{
     			test:  /\.txt$/,
     			use: 'raw-loader'
   			}
     	]
   	}
   }
   ```

   1. Test 属性： 用于匹配需要被处理的文件类型
   2. use 属性： 表示进行转换时， 应该使用哪个loader



5. Plugins

   插件用于bundle文件的**优化**，**资源管理**和**环境变量注入**，作用于**整个构建过程**。

   任何 loader无法完成的事情， 都可以用plugins去实现

   常用plugins

   | 名称                     | 描述                                     |
   | ------------------------ | ---------------------------------------- |
   | CommonsChunkPlugin       | 将chunks相同的模块代码提取成公共js       |
   | CleanWebpackPlugin       | 清理构建目录                             |
   | ExtractTextWebpackPlugin | 将css从bundle文件里提取成一个独立css文件 |
   | CopyWebpackPlugin        | 将文件或者文件夹拷贝到构建的输出目录     |
   | HtmlWebpackPlugin        | 创建html文件去承载输出的bundle           |
   | UglifyjsWebpackPlugin    | 压缩js                                   |
   | ZipWebpackPlugin         | 将打包出的资源生成一个zip包              |

   用法：

   把插件放到plugins 数组中...

   ```javascript
   module.exports = {
     output: {
       filename: ''
     }
     plugins:[
     	new HtmlWebpackPlugin()
     ]
   }
   ```

   

6. Mode

   用来指定当前的构建环境是：production	、 development 还是none

   设置mode可以使用webpack内置的函数，默认值为production

   | 选项        | 描述                                                         |
   | ----------- | ------------------------------------------------------------ |
   | Development | 设置 `process.env.NODE_ENV`  的值为 `development` , 开启 `NamedChunksPlugins` 和 `NamedModulesPlugins` |
   | Production  | 设置 `process.env.NODE_ENV` 的值为 `production`, 开启 `FlagDependencyUsagePlugin`,  `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin`, `OccurrenceOrderPlugin`, `SideEffectsFlagPlugin` 和 `TerserPlugin` |
   | None        | 不开启任何优化选项                                           |

   

7. Babel-loader

   首先，安装 babel 插件

   `npm i @babel/core @babel/preset-env babel-loader -D`

   然后，建立 .babelrc 文件，进行配置

   ```javascript
   .babelrc
   {
     "presets": [
       "@babel/preset-env"
     ]
   }
   ```

   

8. 解析css

   Css-loader： 用于加载 .css文件， 并转换成commonjs 对象

   Style-loader: 将样式通过<style>标签 插入到head中

   同时有多个loader， 是链式调用， 会从最后一个loader往第一个loader执行， 每执行一个会把结果传给上一个loader

   ```javascript
   module: {
           rules: [
               {
                   test: /.js$/,
                   use: 'babel-loader'
               },
               {
                   test: /.css$/,
                   use: [
                       'style-loader',
                       'css-loader'
                   ]
               },
               {
                   test: /.less$/,
                   use: [
                       'style-loader',
                       'css-loader',
                       'less-loader'
                   ]
               }
           ]
       },
   ```

   

9. 解析图片 & 字体资源

   File-loader: 用于处理文件

   Url-loader: 相比file-loader ， 会对小文件 自动做base64转换

   

   File-loader:  图片12k，  search文件133k

   ![屏幕快照 2020-04-26 22.42.06](/Users/murphy/Desktop/屏幕快照 2020-04-26 22.42.06.png)

   

   url-loader： 图片被转成base64，  search增加到149

   ![屏幕快照 2020-04-26 22.44.42](/Users/murphy/Desktop/屏幕快照 2020-04-26 22.44.42.png)



10. 文件监听

    文件指纹概念： 打包后输出**文件名的后缀**

    通常可以用来做版本管理

    3种文件指纹：Hash, Chunkhash, Contenthash

    Hash: 和整个项目的构建相关，只要项目文件有修改，整个项目构建的hash值就会更改

    Chunkhash: 和webpack打包的chunk有关，不同的entry会生成不同的chunkhash值

    Contenthash: 根绝文件内容来定义hash，文件内容不变，则contenthash不变

    js文件指纹：

    图片文件指纹：

    cs s文件指纹

    