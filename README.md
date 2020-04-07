# prerendering
对SEO优化，为了能被搜索引擎搜索到，可被爬虫 ——有利于营销
预渲染：把前端中的页面作为静态站点去渲染，而不是使用服务器去把对应的html渲染到前端，而且要保证是单页面
1.要在路由是mode:'history' 的模式下才可行

2..安装预渲染插件	
   执行：npm i prerender-spa-plugin -D

3.导入插件在build下的环境中(prod.conf.js)
（1）
      const PrerenderSPAPlugin = require('prerender-spa-plugin');

      //调用渲染器
      const Renderer = PrerenderSPAPlugin.PuppeteerRenderer;
（2）
      然后在plugins中添加一下这个插件的配置
      //预渲染插件的配置
      new PrerenderSPAPlugin({
      staticDir:path.join(__dirname,'../dist'),
      routes:['/','/about'],
      //这个配置很重要，如果没有也不会进行预编译
      renderer:new Renderer({
        inject: {
          foo:"far"
        },
        headless:false,
        //在项目入口中使用document.dispatchEvent(new Event('render-event')),要对应下面的名字
        renderAfterDocumentEvent:'render-event'
      })
    })
（3）在main.js中，mounted加载的时候去添加这个预渲染
           mounted(){
   	 document.dispatchEvent(new Event('render-event'))
 	 }

4.使用npm run build打包，此时会生成一个about文件，内部有一个静态的index.html, 本地服务器hs -o -p 8888启动，能够看到对应的SEO信息
