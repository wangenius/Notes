# 环境配置和框架搭建

***
## nodejs与npm包管理工具
[下载地址-官网](https://nodejs.org/en/)  
由于Node.js平台是在后端运行JavaScript代码，所以，必须首先在本机安装Node环境。  
npm是什么东东？npm其实是Node.js的包管理工具（package manager）。

1. [npm npx yarn的区别和联系](https://blog.csdn.net/zheng18237111686/article/details/113933072)

2. npm npx速度慢的问题：
    ```
    npm config set registry https://registry.npm.taobao.org
    -- 配置后可通过下面方式来验证是否成功
    npm config get registry
    -- 或npm info express
    ```
    
### npm packages
1. [Sass](https://www.runoob.com/sass/sass-tutorial.html)  
    ```SSH
    npm install -g sass
    ```
2. [react](https://reactjs.org/docs/create-a-new-react-app.html)
    
    我们建议在 React 中使用 CommonJS 模块系统，比如 browserify 或 webpack，本教程使用 webpack。
    ```
    npx create-react-app my-app

    npm start
    ```
    [react学习笔记](react.md)

    
3. [axios]
4. [redux]
5. 