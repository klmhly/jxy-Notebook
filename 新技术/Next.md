1.   Next 优势

   - 基于页面的直观的路由系统（也支持动态路由）

   - 适时的自动静态优化页面

   - 对数据有大量需求的页面在服务器端渲染

   - 自动代码拆分以加快页面加载速度

   - 带有页面预取功能的客户端路由

   - 内置 CSS 支持，并支持任何 CSS-in-JS 库

   - 基于 Webpack 的开发环境，支持 [模块热替换](https://webpack.js.org/concepts/hot-module-replacement/) (HMR)

   - API routes to build your API with serverless functions, with the same simple router used for pages

   - 可通过社区提供的插件和您自己的 Babel 和 Webpack 配置进行定制

     

2. 模块热替换

   修改代码错误，无需刷新浏览器，页面会立即出现

3. 页面之间跳转

   ```javascript
   <Link  href='/about'><a>链接</a></Link>
   ```

   这是客户端导航。操作在浏览器中进行，而不向服务器发出请求。 您可以通过打开浏览器的网络请求检查器（network request inspector）来验证这一点。

4. `/pages` and `/public` 是特定意义的文件夹

5. 渲染子组件的方式

   1，{props.children}

   2，HOC

   3，页面内容作为prop

6. 推荐 css in js 的形式