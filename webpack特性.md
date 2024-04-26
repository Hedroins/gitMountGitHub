# Webpack基础知识
webpack是一个模块化Javascript工具，它会从入口文件出发，识别出源码中的模块化导入语句，递归的找出所有依赖，将入口和其所有依赖包打包到一个单独的文件中。
可以在源码中指定使用什么loader去处理文件，写法require('style-loader!css-loader!./index.css')
devServer会启动一个服务器用于服务网页请求，同时会帮助启动webpack， 并接受Webpack发出的文件变更信号，通过WebSocket协议自动刷新网页并实时预览。
模块热替换：不重新加载整个网页的情况下，将已更新的模块替换老模块，再重新执行一次来实现实时预览

# Webpack配置

## 入口(entry)
        入口文件，可以是一个文件，也可以是多个文件，还可以是一个对象，对象中可以包含多个入口文件。
        entry可以是string、array、object
        string：入口文件为单个文件，如：entry: './src/index.js' 只会生成一个chunk
        array：入口文件为多个文件，如：entry: ['./src/index.js', './src/index2.js'] 只会生成一个chunk
        object：入口文件为多个文件，如：entry: { main: './src/index.js', main2: './src/index2.js' } 每个入口生成一个chunk
        entry：也可以是一个函数，函数返回一个对象，如：entry: () => ({ main: './src/index.js', main2: './src/index2.js' })


## 输出(output)
   filename  chunkFilename （chunkFileName只用于对运行过程中产生的模块进行命名）path 
   publicPath 配置发布到线上的路径
   crossOriginLoading 配置跨域请求是否允许携带凭证 用于JSONP异步加载在动态插入的script标签上增加crossoigin属性(取值为anonymous使用该脚步时不会带上用户的Cookies, use-credentials加载会带上用户的Cookie)
   library 配置导出库的名称(library和libraryTarget一起使用,用于构建一个被其他模块导入使用的库)
   libraryTarget 以何种方式导出库（
       var 将编写的库通过var 被赋值给library指定名称的变量。例如output.library = 'LibraryName' webapck输入的代码为var LibraryName = lb_code ; LibraryName.doSomething()
       ,commonjs
       ,commonjs2,this,window,global）
   libraryExport 配置要导出的模块中哪些子模块需要被导出。

## 模块(module)
   rules 配置模块的读取和解析规则
       test 配置要匹配的文件类型
       use 配置要应用的loader
           loader 配置要使用的loader
           options 配置loader的参数
           enforce 配置loader的类型(pre:前置，normal:正常，post:后置)
           parser 配置解析规则 ，可以配置哪些语法被解析那些不被解析。
              例如：module:{
                rules:[{
                    test:/\.js$/,
                    use:['babel-loader'],
                    parser:{
                        amd:false, //禁用amd
                        commonjs:false, //禁用commonjs
                        system:false, //禁用system
                        harmony:false, //禁用harmony
                        requireInclude:false, //禁用requireInclude
                        requireEnsure:false, //禁用requireEnsure
                        requireContext:false, //禁用requireContext
                        browserify:false, //禁用browserify
                        requireJs:false, //禁用requireJs
                        node:false //禁用node
                    }
                }]
              }
            
            noParse webpack配置项可以让Webpack忽略对部分没采用模块化的文件的递归解析处理，这样做的好处是能提高构建性能，例如noParse: /jquery|lodash/
               注意被忽略的文件里不应该包含import、require、define等模块化语句，不然会导致构建出的代码中包含无法在浏览器环境下执行的模块化语句。
       exclude 配置要排除的文件类型
       include 配置要进行匹配的文件 （exclude和include数组之间都是或的关系，只是文件路径满足数组的任一元素就会被命中）
     

## 插件(plugins)
   html-webpack-plugin 自动生成一个HTML文件，并把打包生成的js文件自动引入到该HTML文件中
   clean-webpack-plugin 清理构建目录
   copy-webpack-plugin 复制文件到构建目录
   mini-css-extract-plugin 提取css到单独的文件中
     

## 解析(resolve)
   alias 配置解析模块的别名，如：alias: { '@': path.resolve(__dirname, 'src/') }
   extensions 在导入语句没有带文件后缀时，webpack会自动带上后缀后去尝试访问文件是否存在。
   noParse 配置哪些文件不需要进行解析
   modules:配置去哪里寻找第三方模块，默认值为['node_modules']
   mainFiles:webpack会根据mainFields的配置去决定优先采用那份代码，mainFields默认如下:mainFields:['browser', 'module', 'main']
   descriptionFiles:配置描述第三方模块的文件名称。
   enforceExtenions:配置是否强制使用文件后缀
   enforceModuleExtension:配置是否强制使用文件后缀,与enforceExtenions类似的区别在于enforceModuleExtension是针对第三方模块的(node_modules下的)
   symlinks:是否根据软链接去搜寻文件路径。

## 开发工具(devtool)
   source-map 生成一个独立的source-map文件
   eval-source-map 生成一个eval函数包裹的source-map文件，不生成单独的source-map文件
   cheap-source-map 生成一个不带列映射的source-map文件，不生成单独的source-map文件
   resolveLoader:配置去哪里寻找loader
   resolveLoader:{
       modules:[path.resolve(__dirname, 'node_modules'), path.resolve(__dirname, 'loaders')],
       extensions:['.js', '.json'],
       mainFiles:['index'],
       descriptionFiles:['package.json'],
       enforceExtenions:false,
       enforceModuleExtension:false
   }
   include 配置需要进行解析的文件
   exclude 配置不需要进行解析的文件
   mainFiles:webpack会根据mainFields的配置去决定优先采用那份代码，mainFields默认如下:mainFields:['browser', 'module', 'main']

## 解析(resolve)
   alias 配置解析模块的别名，如：alias: { '@': path.resolve(__dirname, 'src/') }
   extensions 配置省略文件后缀名
   noParse 配置哪些文件不需要进行解析
   include 配置需要进行解析的文件
   exclude 配置不需要进行解析的文件
   parser 配置解析规则

## 插件(plugins)
   html-webpack-plugin 自动生成一个HTML文件，并把打包生成的js文件自动引入到该HTML文件中
 


## 加载器(loader)

## 插件(plugins)

## 模式(mode)
   development:开发模式
   production:生产模式
## 开发工具(devServer)
  hot:是否启用模块热替换
  inline: DevServer的实时预览功能依赖一个在页面里注入的代理客户端，inline配置是否将这个代理客户端自动注入运行页面的chunk里，默认自动注入，如果开启inline则用代理客户端控制网页刷新。否则使用iframe的方式构建开发的网页，通过刷新iframe来实现实时预览。
  port:指定DevServer服务器的端口号
  host:指定DevServer服务器的IP地址，如果想让局域网的其他设备访问到服务器，可以设置为0.0.0.0
  allowedHosts:配置允许访问DevServer服务器的IP地址，默认允许所有ip访问。
  disableHostCheck:是否禁用主机检查
  https:是否启用HTTPS协议
  open:是否自动打开浏览器
  proxy:配置代理服务器
  historyApiFallback:是否启用HTML5 History API HistoryApiFallback的配置项
    rewrites:配置URL重写规则
    disableDotRule:是否禁用点号前缀的匹配规则
  contentBase:指定DevServer服务器的文件根目录, 只能用来暴露本地文件，因为devServer构建出的内容在服务器的内存中，可以设置contentBase：false关闭暴露本地内容。
  headers:在HTTP服务器响应时注入一些响应头信息。
  watchContentBase:是否监视contentBase目录下的文件
  watchOptions:配置文件监视选项
    ignored:配置不监视的文件
    poll:是否启用轮询
    ignored:配置不监视的文件
    poll:是否启用轮询
  compress:是否启用gzip压缩

## 性能优化
   1、缩小文件的查找范围 
        优化loader配置，增加include配置只命中需要处理的文件
        优化resolve.modules配置，直接指明第三方工具库的存放路径，减少不必要的查找。
        优化mainFields,这个值的默认值和target配置有关，如果target为web或者webworker则mainFields默认值为['browser', 'module', 'main']，如果target为node则mainFields默认值为['module', 'main']。
        为了减搜索步骤在明确第三方模块的入口文件描述字段时，将其设置得尽可能少。
        优化resolve.alias，减少文件的查询路径。
        优化resolve.extensions，减少文件后缀尝试次数。
        优化resolve.cacheWithContext，是否缓存文件查找结果。
        优化module.noParse，不需要解析的文件。
        优化module.unsafeCache，不需要缓存的文件。
        优化module.unknownContextCritical，不安全的上下文。

   2、使用DllPlugin和DllReferencePlugin优化打包速度
        DllPlugin:将第三方库单独打包，避免每次构建都重新打包第三方库。
        DllReferencePlugin:在配置文件中引入DllPlugin打包好的第三方库。
   3、使用HappyPack在执行loader时,将任务分解给多个子进程去并发执行，子进程处理完后再将结果发送给主进程。HappyPack的配置项如下：
       threadPool:配置线程池，可以指定一个已存在的线程池，也可以自己创建一个线程池。
       id:配置HappyPack的标识符，每个HappyPack的实例都有一个唯一的标识符。
       loaders:配置HappyPack要使用的loader。
       verbose:是否允许输出日志信息。

   4、使用ParallelUglifyPlugin,将压缩JS代码任务分解给多个子进程去并发执行，子进程处理完后再将结果发送给主进程。


   5、watchOptions poll越小越好（每秒轮询次数）. aggregateTimeout越大越好（文件发生变化后，等待多久重新构建）

   6、使用Tree Shaking,Tree Shaking的原理是：在打包过程中，会分析模块之间的依赖关系，找到模块之间没有引用关系的代码，然后将这部分代码删除，从而实现打包结果的优化。

   7、使用Scope Hoisting,Scope Hoisting的原理是：在打包过程中，会将所有的模块代码按照引用顺序放在一个函数作用域里，然后适当的重命名一些变量以防止变量名冲突。
      使用方法：
         optimization:{
             usedExports:true, // 开启Tree Shaking
             concatenateModules:true, // 开启Scope Hoisting
         }
         

   
   8、definePlugin:配置全局常量,

   9、压缩CSS代码PostCss

   10、提取公共代码CommonChunkPlugin, 用法new CommonChunkPlugin({
       name: 'common', //提取出的公共部分形成的一个新的chunk名称
       chunks: ['a','b'], //从哪些chunk中提取代码
       minChunks?:1
   })

   11、提取公共代码SplitChunksPlugin, 用法new SplitChunksPlugin({
       chunks: 'all',
       minSize: 30000, // 模块超过30k会被提取到公共模块
       minChunks: 1, // 模块至少被引用1次
       maxAsyncRequests: 5, // 按需加载时候最大的并行请求数
   })

   12、webpackAnalyse:分析打包结果
 

## 代码分割

## 缓存

## 环境变量

## 模块联邦

## 代码校验

## 代码压缩

## 其他
  context:webpack在寻找相对路径时会以context为根目录，context默认为执行启动webpack的当前工作目录。context必须是一个绝对路径的字符串(可以通过path.resolve(__dirname, 'src')来获取
  Target:配置项可以让webpack针对不同环境构建不同的代码，如针对浏览器web， 针对nodejs node  针对webworker webworker ,针对electron electron-main 等

  devtool:配置项可以让webpack在打包时生成source-map文件，用于定位代码错误位置，devtool的配置项如下：
    source-map:生成一个独立的source-map文件，这个文件可以定位到源代码的错误位置
    eval-source-map:生成一个eval函数包裹的source-map文件，这个文件可以定位到源代码的错误位置
    cheap-source-map:生成一个不带列映射的source-map文件，这个文件可以定位到源代码的错误位置

  externals:配置项可以让webpack忽略掉一些不需要打包的模块，如：externals: { jquery: 'jQuery' }

  watch:配置项可以让webpack在文件发生变化时自动重新构建
  watchOptions:配置项可以让webpack在文件发生变化时自动重新构建，配置项如下：
    poll:是否启用轮询，每秒轮询次数。
    aggregateTimeout:文件发生变化后，等待多久重新构建
  reolveLoader 告诉webpack去哪里寻找loader.
  resolveLoader:{
      modules:[path.resolve(__dirname, 'node_modules'), path.resolve(__dirname, 'loaders')], //去哪个目录下寻找loader
      extensions:['.js', '.json'], //入口文件的后缀
      mainFiles:['index'],
      descriptionFiles:['package.json'],
  }

## 多配置情况
1、可以导出一个函数，根据环境变量来区分。
2、webpack.config.js 可以导出一个数组，每个元素代表不同环境的配置。


## stage等级含义：
stage0 只是一个美好激进的想法，一些Babel实现了对这些特性的支持，不确定会不会定为标准。
stage1 值得被纳入标准的特性
stage2 已经起草，将要被纳入标准
stage3 已定定稿，各大浏览器着手实现
stage4 在接下来的一年里将会加入到标准里


## webpack 扩展
1、如果提取组件的公共代码？
2、Service Workers的使用。 可以通过navigator.serviceWorker判断是否支持serviceWorker
   ServiceWorkers的应用背景缓存。 ServiceWorkers只有在HTTPS下才能正常工作。
4、webpack-dev-middleware 可以基于现有的服务器实现devServer
5、file-loader可以将js、css导入图片替换为正确的地址
6、url-loader可以将js、css导入图片替换为正确的地址，并且可以设置图片大小，小于设置的值则转换为base64，limit选项可以控制图片的限制，如果大于limit会用fallback指定的loader，如果小于limit
   会用url-loader来处理图片。
7、加载svg 可能用到的loader raw-loader file-loader svg-inline-loader(和raw-loader类似，但是可以处理svg,去除无关的节点)
8、loader本质上是导出的一个函数,第一个参数是处理前的源码，在loader中获取用户传的参数let options = loaderUtils.getOptions(this);
   webpack向loader提供的通信API，在loader中调用this.callback(err, content, sourceMap, meta)可以返回处理后的结果。调用this.sourceMap获取sourceMap信息，调用this.emitFile(name, content)可以生成一个文件。
9、异步loader 调用this.async()可以返回一个Promise对象，在Promise对象中调用this.callback(err, content, sourceMap, meta)可以返回处理后的结果。

10、loader的this对象
   this.query:获取用户传的参数
   this.context:处理的当前文件所在的目录
   this.callback:返回处理后的结果
   this.async:返回一个Promise对象
   this.cacheable:是否缓存 // this.cacheable(false)关闭缓存
   this.addDependency:添加依赖
   this.addContextDependency:添加上下文依赖
   this.resource:获取当前处理的文件的完整请求路径
   this.resourcePath:获取当前处理的文件的路径
   this.resourceQuery:获取当前处理的文件的查询字符串
   this.sourceMap:获取当前处理的文件的sourceMap
   this.emitFile:生成一个文件
   this.fs:获取文件系统
   this.resolve:解析模块路径
   this.options:获取用户配置的options
   this.loadModule:在Loader处理一个文件时，如果依赖其他文件的处理结果，可以通过this.loadModule(request: string, callback: function(err, source, sourceMap, module))来加载其他文件的处理结果。
   this.exec:执行一个loader

11、loader获取webpack传递的二进制数据：
    loader里写上 exports.raw = true;

12、loader获取webpack传递的源码数据：
     exports.source = true;

13、loader获取webpack传递的源码数据和二进制数据：
     exports.raw = true;
     exports.source = true;

14、loader获取webpack传递的源码数据和二进制数据：
     exports.raw = true;
     exports.source = true;

15、loader获取webpack传递的源码数据和二进制数据：

