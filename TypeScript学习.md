默认情况下，TS会做出以下假设：
1、假设当前的执行环境是dom
2、如果代码中没有使用模块化语句(import、export),便以为该代码是全局运行
3、编译的目标代码是ES3

有两种方式更改以上假设：
1、使用tsc命令行编译时，加上 --target参数指定编译后的ES版本
2、使用tsconfig.json配置文件，指定编译选项

配置文件tsconfig.json 通过tsc --init生成

配置文件tsconfig.json
{
    "compilerOptions":{
        "target":"ES6",  //编译后的目标代码是ES6
        "module":"commonjs", //编译后的代码使用模块化规范
        "lib":["dom","ES6"],  //编译时需要包含的库，需要包含node环境安装@types/node
        "outDir":"./dist",
        "rootDir":"./src",
        "strict":true
    },
    "include":[
        "./src/**/*"   //需要编译的目录，避免编译不必要的文件
    ],
    "files":[
        "./src/index.ts"   //指定需要编译的文件
    ],
    "exclude":[
        "./src/test/**/*"
    ]
}
使用了配置文件后，编译时只需要使用tsc命令即可，不需要加参数
@types是ts官方的类型库，其中包含了很多js代码的类型描述

ts-node是ts官方提供的支持ts代码直接运行的库，不需要编译成js代码再运行
安装ts-node后，可以直接使用ts-node index.ts运行ts代码

nodemon:监听文件变化，自动重启node服务安装nodemon后，nodemon --watch -e ts --exec ts-node index.ts运行ts代码

基本类型：
number、string、boolean、数组（number[]、string[],boolean[],Array<number>）、
object 
null、undefined 是所有其他类型的子类型，可以赋值给其他类型
通过添加strictNullChecks:true标记，null和undefined只能赋值给void和它们各自



