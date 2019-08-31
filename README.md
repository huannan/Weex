#### 1.1 常见跨平台开发方案对比

#### 1.2 Weex基本介绍

![Weex](https://upload-images.jianshu.io/upload_images/2570030-f33ad2e43aacf55a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Weex是使用流行的Web开发体验来开发高性能原生应用的框架。
* Weex致力于使开发者能基于通用跨平台的 Web开发语言和开发经验，来构建Android、iOS和Web应用。简单来说，在集成了WeexSDK之后，你可以使用JavaScript语言和前端开发经验来开发移动应用。
* Weex渲染引擎与DSL语法层是分开的，Weex并不强依赖任何特定的前端框架。目前```Vue.js```和```Rax```这两个前端框架被广泛应用于Weex页面开发，同时Weex也对这两个前端框架提供了最完善的支持。Weex的另一个主要目标是跟进流行的Web开发技术并将其和原生开发的技术结合，实现开发效率和运行性能的高度统一。在开发阶段，一个Weex页面就像开发普通网页一样；在运行时，Weex页面又充分利用了各种操作系统的原生组件和能力。

#### 1.3 Weex开发环境搭建

1. 到Node.js官网下载安装包，安装Node.js环境（OSX环境环境默认自带Node.js），并且给自带的node_modules目录赋予可执行权限：
    ```bash
    sudo chmod -R 777 /usr/local/lib/node_modules/
    ```
2. 通过Node.js的包管理工具npm安装weex-toolkit，weex-toolkit是一个脚手架工具来辅助开发和调试
    ```bash
    npm install -g cnpm --registry=https://registry.npm.taobao.org // 在天朝可以使用npm的淘宝镜像cnpm代替npm
    cnpm i -g weex-toolkit // 安装不要使用sudo执行，-g表示全局安装
    ```
3. 通过检查Weex环境的版本号，确定开发环境是否搭建好：
    ```bash
    weex -v // 查看当前weex工具版本
    ```
    
#### 1.4 Weex项目创建与编译运行

1. 创建项目：
    ```bash
    weex create project-name
    ```
2. 切到项目目录，即项目对应的package.json文件所在的目录，安装项目所需的依赖：
    ```bash
    npm install
    ```
3. 编译项目，检查项目是否编译通过：（如果编译不过，请查看下面的常见问题与解决办法）
    ```bash
    npm run build
    ```
4. 运行项目：
    ```bash
    npm run weex 或者 npm run serve
    ```
5. 开启代码热更新：
    ```bash
    npm run dev
    ```  
    
编译不过常见解决办法：

* 大部分情况都是因为依赖没有安装完整，尝试多次```npm install```和```cnpm install```
* 如果提示显示找不到node_modules/node-sass/vendor模块，尝试```npm rebuild node-sassl```