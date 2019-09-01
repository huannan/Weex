#### 1.1 常见跨平台开发方案对比

#### 1.2 Weex基本介绍

* Weex是使用流行的Web开发体验来开发高性能原生应用的框架。
* Weex致力于使开发者能基于通用跨平台的 Web开发语言和开发经验，来构建Android、iOS和Web应用。简单来说，在集成了WeexSDK之后，你可以使用JavaScript语言和前端开发经验来开发移动应用。
* Weex渲染引擎与DSL语法层是分开的，Weex并不强依赖任何特定的前端框架。目前```Vue.js```和```Rax```这两个前端框架被广泛应用于Weex页面开发，同时Weex也对这两个前端框架提供了最完善的支持。Weex的另一个主要目标是跟进流行的Web开发技术并将其和原生开发的技术结合，实现开发效率和运行性能的高度统一。在开发阶段，一个Weex页面就像开发普通网页一样；在运行时，Weex页面又充分利用了各种操作系统的原生组件和能力。

![Weex](https://upload-images.jianshu.io/upload_images/2570030-f33ad2e43aacf55a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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

#### 1.5 使用Weex Studio

![Weex Studio](https://upload-images.jianshu.io/upload_images/2570030-99c30f6e79ef5679.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 集成了所需要的开发工具，省去了环境的搭建过程
* 创建项目更加方便
* 提供了预览窗口
* 目前仅支持Windows、OSX
* ......

#### 1.6 添加不同平台的项目

1. 默认情况下```weex create```命令并不初始化iOS和Android项目，你可以通过执行```weex platform add```来添加特定平台的项目：
    ```bash
    weex platform add ios
    weex platform add android
    ```
2. 配置好客户端的开发环境，比如Android就需要安装配置好Android Studio
3. 开发环境准备就绪后，运行下面的命令，可以在模拟器或真实设备上启动应用：
    ```bash
    weex run ios
    weex run android
    weex run web
    ```
4. 调试：
    ```bash
    weex debug
    ```
    
#### 1.7 集成到Android项目

1. 添加依赖：
```groovy
dependencies {
    ...
    // weex sdk and fastjson
    implementation 'com.taobao.android:weex_sdk:0.20.0.2@aar'
    implementation 'com.alibaba:fastjson:1.1.46.android'

    //support library dependencies
    implementation 'com.android.support:recyclerview-v7:23.1.1'
    implementation 'com.android.support:support-v4:23.1.1'
    implementation 'com.android.support:appcompat-v7:23.1.1'
}
```
    
2. 配置混淆规则：
```proguard
-keep class com.taobao.weex.bridge.** { *; }
-keep class com.taobao.weex.layout.** { *; }
-keep class com.taobao.weex.WXSDKEngine { *; }
-keep class com.taobao.weex.base.SystemMessageHandler { *; }
-dontwarn com.taobao.weex.bridge.**
```

3. 声明权限：
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

4. 在Application中初始化SDK：

```java
InitConfig config = new InitConfig.Builder()
					//图片库接口
    				.setImgAdapter(new FrescoImageAdapter())
    				//网络库接口
    				.setHttpAdapter(new InterceptWXHttpAdapter())
    				.build();
WXSDKEngine.initialize(applicationContext,config);
```

5. 创建WXSDKInstance

* WXSDKInstance是weex渲染页面的基本单元（PS：不同页面会有不同的WXSDKInstance，因此全局事件不能使用instance.fireEvent()，而是使用Broadcast Channel）
* 通过instance.render(url)拉取js bundle
* 在回调IWXRenderListener的onViewCreated返回创建的view
* 将返回的view添加到布局容器中，例如Activity的view上（rootView）

```java
public class MainActivity extends AppCompatActivity implements IWXRenderListener {
    
    private WXSDKInstance mWXSDKInstance;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mWXSDKInstance = new WXSDKInstance(this);
        mWXSDKInstance.registerRenderListener(this);
       
        String pageName = "WXSample";
        String bundleUrl = "http://dotwe.org/raw/dist/38e202c16bdfefbdb88a8754f975454c.bundle.wx";
        mWXSDKInstance.renderByUrl(pageName, bundleUrl, null, null, WXRenderStrategy.APPEND_ASYNC);
    }

    @Override
    public void onViewCreated(WXSDKInstance instance, View view) {
        setContentView(view);
    }

    @Override
    public void onRenderSuccess(WXSDKInstance instance, int width, int height) {
    }

    @Override
    public void onRefreshSuccess(WXSDKInstance instance, int width, int height) {
    }

    @Override
    public void onException(WXSDKInstance instance, String errCode, String msg) {
    }

    @Override
    protected void onResume() {
        super.onResume();
        if (mWXSDKInstance != null) {
            mWXSDKInstance.onActivityResume();
        }
    }

    @Override
    protected void onPause() {
        super.onPause();
        if (mWXSDKInstance != null) {
            mWXSDKInstance.onActivityPause();
        }
    }

    @Override
    protected void onStop() {
        super.onStop();
        if (mWXSDKInstance != null) {
            mWXSDKInstance.onActivityStop();
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (mWXSDKInstance != null) {
            mWXSDKInstance.onActivityDestroy();
        }
    }
}
```

6. 运行项目，查看效果

#### 2.1 Vue基本介绍

* Vue.js是当下很火的一个JavaScript MVVM库，它是以数据驱动和组件化的思想构建的。相比于Angular.js，Vue.js提供了更加简洁、更易于理解的API，使得我们能够快速地上手并使用Vue.js。
* 如果你之前已经习惯了用jQuery操作DOM，学习Vue.js时请先抛开手动操作DOM的思维，因为Vue.js是数据驱动的，你无需手动操作DOM。它通过一些特殊的HTML语法，将DOM和数据绑定起来。一旦你创建了绑定，DOM将和数据保持同步，每当变更了数据，DOM也会相应地更新。

![Vue](https://upload-images.jianshu.io/upload_images/2570030-02f332dd4c21f14a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.2 理解Vue的MVVM模式

![Vue的MVVM](https://upload-images.jianshu.io/upload_images/2570030-cdfdd9eb27f41968.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* ViewModel是Vue.js的核心，它是一个Vue实例。Vue实例是作用于某一个HTML元素上的，这个元素可以是HTML的body元素，也可以是指定了id的某个元素。
* 上图中的DOM Listeners和Data Bindings看作两个工具，它们是实现数据双向绑定的关键：
    1. 从Model侧看，当我们更新Model中的数据时，Data Bindings工具会帮我们更新页面中的DOM元素。
    2. 从View侧看，ViewModel中的DOM Listeners工具会帮我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据。

最简单的示例：

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Demo</title>
</head>

<body>
<!--这是View-->
<div id="app">
    <p>{{message}}</p>
    <input type="text" v-model="message"/>
</div>
</body>

<script src="../js/vue.js"></script>
<script>
    //这是ViewModel
    let vue = new Vue({
        el: '#app',
        //这是Model
        data: {
            message: 'Hello World!'
        }
    });
</script>
</html>
```

#### 2.3 Vue的常用指令

##### 2.3.1 什么是Vue指令

Vue.js的指令是以v-开头的，它们作用于HTML元素，指令提供了一些特殊的特性，将指令绑定在元素上时，指令会为绑定的目标元素添加一些特殊的行为，我们可以将指令看作特殊的HTML特性（attribute）。

* Vue.js提供了一些常用的内置指令
    * v-if指令
    * v-show指令
    * v-else指令
    * v-for指令
    * v-bind指令
    * v-on指令
* Vue.js具有良好的扩展性，我们也可以开发一些自定义的指令

##### 2.3.2 v-if指令

https://www.cnblogs.com/keepfool/p/5619070.html