# webpack源码阅读

### 1、webpack从lib/index.js文件启动。执行lazyFunction(()=>require("./webpack")),返回包含各种功能属性的匿名

### 函数。在执行这个匿名函数，会先进行配置项校验。校验是将webpack的schema和用户配置的config对象进行比对。如果不满足

### schema定义的格式，会根据不同错误的类型抛出。（schema-utils工具包，validate方法的第三个参数对象，name属性：工具

### 名，baseDataPath：默认值为configuration, postFormatter函数：第一个参数formattedError,第二个参数

### error.children可以分辨错误类型，根据不同类型对于不同错误提示）


###2、如果配置校验通过。创建compiler。如果传入的配置是几份不同的配置，（config文件导出的是数组）， 则调用

### createMultiCompiler ，createMutiCompiler遍历每一个配置项，调用createCompiler。 createCompiler调用

### getNormalizedWebpackOptions（./config/normalization.js）对配置项进行归一化(对用户配置中没有写到的配置进行

### 赋默认值)。 归一化完成调用applyWebpackOptionsBaseDefaults。（设置配置项的context值为process.cwd()，对日志

### 进行设置)， 然后创建Compiler类的实例 。 Compiler的构造函数接受两个参数，一个是context 值为当前工作目录。另一

### 个是配置对象，然后返回compiler实例，接着构造NodeEnvironmentPlugin插件实例，为compiler实例绑定inputFileSystem

### (enhanced-resolve工具包中的CachedInputFileSystem实例)，绑定outputFileSystem。 绑定intermediateFileSystem。

### 绑定watchFileSystem 。 然后在hooks.beforeRun注册（tap）NodeEnvironmentPlugin事件。（用于清掉inputFileSystem

### 缓存的数据）接着安装插件，遍历配置对象的plugins数值，如果插件返回的是一个函数，用.call方法将当前创建的compiler

### 实例，绑定为插件函数的上下文，并且传入compiler实例。如果插件是一个对象实例，则调用它的apply方法，将插件实例作为

### 参数传给apply方法。（这样在插件内部可以调用compiler的各种钩子，对打包的内容进行处理。 组件安装完成后，调用了

### applyWebpackOptionsDefaults函数将配置对象作为参数，这个方法的主要功能是，为配置对象增加context属性，（利

### 用nodejs process.cwd()调用获得），增加target属性，底层是通过browserslist工具包的loadConfig方法和

### findConfig来获取在项目根目录下是否有.browerslistrc配置，或者在package.json中是否配置了browerslist字段

### 如果配置了则会将target 设置为'browserslist' 否则设置为'web'。接着调用了environment钩子和afterEnvironment

### 钩子


