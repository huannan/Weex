![Weex从入门到放弃](https://upload-images.jianshu.io/upload_images/2570030-2415165f8bef3675.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 1. Weex基本介绍与开发环境搭建

#### 1.1 常见跨平台开发方案对比

![Flutter、Weex、React Native和Android原生对比](https://img-blog.csdn.net/20180927194910311?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2pvaG5XY2hldW5n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

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
    
    // 直接使用Creator SDK
    // Creator SDK针对原生的Weex SDK进行了大量优化和扩展，如：新增共享元素动画，列表动画，动画属性控制，提供多种样式的系统弹框和Flyme风格的主题组件。用js也可以开发出在体验上和原生native一致的效果
    implementation "com.meizu.creator.commons:creatorsdk:${versions.flyme_creator}"
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

### 2. Weex需要的Vue核心知识入门

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

```html
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
    * v-model指令
* Vue.js具有良好的扩展性，我们也可以开发一些自定义的指令

##### 2.3.2 v-if和v-show指令

* v-if是条件渲染指令，它根据表达式的真假来删除和插入元素，基本语法如下：

```html
v-if="expression"
v-show="expression"
```

* expression是一个返回bool值的表达式，表达式可以是一个bool属性，也可以是一个返回bool的运算式、方法。
* v-if指令是根据条件表达式的值来执行元素的插入或者删除行为
* v-show也是条件渲染指令，和v-if指令不同的是，使用v-show指令的元素始终会被渲染到HTML，它只是简单地为元素设置CSS的style属性（style="display:none"）

##### 2.3.3 v-else指令

* 可以用v-else指令为v-if或v-show添加一个“else块”。v-else元素必须立即跟在v-if或v-show元素的后面——否则它不能被识别。
* 示例如下：

```html
<h1>v-if指令</h1>
<h1 v-if="age > 18">小楠：我大于18岁</h1>
<h1 v-else>小楠：我小于18岁</h1>
<hr/>

<h1>v-show指令</h1>
<h1 v-show="age > 18">小楠：我大于18岁</h1>
<h1 v-else>小楠：我小于18岁</h1>
<hr/>
```

* 另外还有v-else-if指令

##### 2.3.4 v-for指令

* v-for指令基于一个数组渲染一个列表，它和JavaScript的遍历语法相似：

```html
v-for="item in items"
```

* items是一个数组，item是当前被遍历的数组元素。

##### 2.3.5 v-bind指令

* v-bind指令可以在其名称后面带一个参数，中间放一个冒号隔开，这个参数通常是HTML元素的特性（attribute），例如：v-bind:class
* 基本语法如下：

```html
v-bind:argument="expression"
:argument="expression"
```

##### 2.3.6 v-on指令

* v-on指令用于给监听DOM事件，它的用语法和v-bind是类似的，例如监听<a>元素的点击事件
* 示例：

```html
<a v-on:click="doSomething">
<a @click="doSomething">
```

##### 2.3.7 v-model指令

* MVVM模式本身是实现了双向绑定的，在Vue.js中可以使用v-model指令在表单元素上创建双向数据绑定。
* v-model可以实现View中的数据（一般都是值value属性的值）动态跟model（即Vue中的data）绑定，示例如下：

```html
<input type="text" v-model="message"/>
```

* 上面的例子中，用户改变input的内容，那么message也会实时跟着变化，类似于给EditText添加了TextChangedListener一样

### 2.4 Vue的生命周期

* 每个Vue实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到DOM并在数据变化时更新DOM等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。
* 不需要立马弄明白所有的东西，不过随着你的不断学习和使用，它的参考价值会越来越高。

![Vue的生命周期](https://cn.vuejs.org/images/lifecycle.png)

![Vue的生命周期](https://segmentfault.com/img/bV9x3u?w=835&h=544)

### 2.5 Vue的组件（Component）

* 组件系统是Vue.js其中一个重要的概念，它提供了一种抽象，让我们可以使用独立可复用的小组件来构建大型应用，任意类型的应用界面都可以抽象为一个组件树：

![Vue的组件](https://upload-images.jianshu.io/upload_images/2570030-bee912711f4232d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 组件可以扩展HTML元素，封装可重用的HTML代码，我们可以将组件看作自定义的HTML元素。

### 3.1 Weex中的基本概念

![Weex扩展机制](https://upload-images.jianshu.io/upload_images/2570030-1832a3877167ea52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3. Weex的组件和模块入门

### 3.2 Weex的组件（Component）

* Weex提供了一些列的内置组件给我们使用，这些组件都可以跟Android的控制做类比：

![Weex内置组件](https://upload-images.jianshu.io/upload_images/2570030-649d94f3993ee68a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Creator SDK中也提供了一下扩展组件给我们使用：

![Creator SDK中的扩展组件](https://upload-images.jianshu.io/upload_images/2570030-efe65e165f5cc1c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

组件的使用参考链接：

[官方文档](https://weex.apache.org/zh/docs/components/a.html)
[Creator SDK官方文档](http://apps.flyme.cn/docs/book.html?bookId=59df3445a5a0a611eee9f119&doc=59e9640c67e2274f086396a3)

### 3.3 自定义组件（Component）

当原生组件不能满足我们，就需要自定义组件，下面我们结合自定义组件来介绍组件的使用，自定义组件的基本步骤如下。

* 实现自定义控件
* 示例如下（已省略无关代码）：

```java
public class FmRefreshAnimView extends View {

    public void setProgress(float progress) {

    }

    public void start() {

    }

    public void stop() {

    }
}
```

* 继承WXComponent，实现initComponentHostView方法，返回你的自定义控件
* 可以通过@JSMethod注解对外提供JS方法
* 可以通过@WXComponentProp注解对外提供JS属性，其中参数name是指属性名称
* 示例如下（已省略无关代码）：

```java
public class FmRefreshAnim extends WXComponent<FmRefreshAnimView> {

    public FmRefreshAnim(WXSDKInstance instance, WXDomObject dom, WXVContainer parent) {
        super(instance, dom, parent);
    }

    @Override
    protected FmRefreshAnimView initComponentHostView(@NonNull Context context) {
        return new FmRefreshAnimView(context);
    }

    @JSMethod
    public void start() {
        FmRefreshAnimView hostView = getHostView();
        if (null != hostView) {
            getHostView().start();
        }
    }

    @JSMethod
    public void stop() {
        FmRefreshAnimView hostView = getHostView();
        if (null != hostView) {
            getHostView().stop();
        }
    }

    @WXComponentProp(name = "progress")
    public void setProgress(float progress) {
        FmRefreshAnimView hostView = getHostView();
        if (null != hostView) {
            getHostView().setProgress(progress);
        }
    }
}
```

* 在Application初始化的时候注册组件
* 示例如下（已省略无关代码）：

```java
SDKEngine.asyncInitialize(sInstance, config, new SDKEngine.InitListener() {
    @Override
    public void onSuccess() {
        //初始化成功
        try {
            SDKEngine.registerComponent("FmRefreshAnim", FmRefreshAnim.class);
        } catch (WXException e) {
            e.printStackTrace();
        }
    }
});
```

* 在template标签中需要的地方放置你的组件，并且设置ref引用，注意一定要给组件设置样式，决定组件的宽高跟布局位置。
* 可以给自定义组件设置属性，或者调用组件对外提供的JS方法。
* 示例如下（已省略无关代码）：

```html
<template>
    <div>
        <FmRefreshAnim ref="refresh" class="refresh" :progress="progress"></FmRefreshAnim>
    </div>
</template>

<style>
    .refresh {
        width: 34px;
        height: 34px;
    }
</style>

<script>
    export default {
        data() {
            return {
                progress: 0,
            };
        },
        methods: {
            /**
             * 设置下拉新动画进度
             * @param progress 进度0-99
             */
            setProgress(progress) {
                this.progress = progress;
            },
            /**
             * 开始刷新,循环播放
             */
            start() {
                this.$refs.refresh.start();
            },
            /**
             * 停止刷新,停止动画
             */
            stop() {
                this.$refs.refresh.stop();
            },
    };
</script>
```

### 3.3 Weex的模块（Module）

* Weex提供了一些列的内置模块给我们使用：

![Weex内置模块](https://upload-images.jianshu.io/upload_images/2570030-f2165333e332b017.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Creator SDK中也提供了一下扩展模块给我们使用：

![Creator SDK中的扩展模块](https://upload-images.jianshu.io/upload_images/2570030-484f64a59ff931c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

组件的使用参考链接：

[官方文档](https://weex.apache.org/zh/docs/modules/animation.html)
[Creator SDK官方文档](http://apps.flyme.cn/docs/book.html?bookId=59df3445a5a0a611eee9f119&doc=59df4a6267e2271c7fc46b38)

### 3.4 自定义模块（Module）

* 继承WXModule，通过添加@JSMethod注解对外提供JS方法，其中uiThread是指定执行的线程是否为UI线程，默认为true
* 如果是复杂的参数，可以通过JSON来传递
* 示例如下（已省略无关代码）：

```java
public class TestModule extends WXModule {

    public static final String TAG = "TestModule";


    @JSMethod(uiThread = false)
    public List<String> query(String tableName, int limit, int offset) {

    }


    @JSMethod(uiThread = false)
    public void insert(String tableName, String jsonStr) {

    }

    @JSMethod(uiThread = true)
    public void showToast(String message) {
        Toast.makeText(mWXSDKInstance.getContext(), message, Toast.LENGTH_SHORT).show();
    }

}
```

* 在Application初始化的时候注册模块
* 示例如下（已省略无关代码）：

```java
SDKEngine.asyncInitialize(sInstance, config, new SDKEngine.InitListener() {
    @Override
    public void onSuccess() {
        //初始化成功
        try {
            SDKEngine.registerModule("test", TestModule.class);
        } catch (WXException e) {
            e.printStackTrace();
        }
    }
});
```

* 在Vue里面使用模块
* 示例如下（已省略无关代码）：

```js
var test = weex.requireModule('test');
test.showToast('Hello World');
```
