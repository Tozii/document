# 使用node制作命令行工具
---
### 依赖包

- chalk：[美化log输出](https://github.com/chalk/chalk)
- yargs：[处理命令行参数](https://github.com/yargs/yargs)

### 开发

- 创建项目文件夹`mkdir demo-cli`
- 进入项目目录`cd ./demo-cli`,使用`npm init`初始化package.json配置文件
- 添加依赖包(依赖包可以不添加)：
    - `npm install chalk`
    - `npm install yargs`
- 添加启动文件`touch index.js`

### 文件内容

1.package.json
```json
{
  "name": "demo-cli",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "bin": {  //key：配置命令名称，value：命令对于启动文件路径
    "democli": "./index.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "chalk": "^4.1.0",
    "yargs": "^16.0.3"
  }
}
```

2.index.js

第一行`#!/usr/bin/env node`表示该文件内容使用node解释执行，所以在执行文件时不需要使用`node index.js`命令执行,直接在当前文件夹下`./index.js`就可以执行文件
```js
#!/usr/bin/env node
require('yargs') // eslint-disable-line
  .command('serve [port]', 'start the server', (yargs) => {
    yargs
      .positional('port', {
        describe: 'port to bind on',
        default: 5000
      })
  }, (argv) => {
    if (argv.verbose) console.info(`start server on :${argv.port}`)
    serve(argv.port)
  })
  .option('verbose', {
    alias: 'v',
    type: 'boolean',
    description: 'Run with verbose logging'
  })
  .argv
```

### 全局安装测试

- 在项目目录下使用`npm link`命令，该命令会在node_module全局配置路径文件加下创建一个快捷方式，并生成bash、cmd、powershell三种格式的命令脚本
- 将node_module全局配置路径加入环境变量，如果已经加入则可忽略(这步主要是可以在任何地方打开命令行窗口都可以使用`democil`命令)
- 卸载全局测试快捷方式`npm unlink`