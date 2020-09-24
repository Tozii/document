# 使用node制作命令行工具

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