# neargoos 前端工程

## 安装

```
1.控制台依次安装nodejs，npm(新版nodejs自带), express，cnpm, vue-cil,webpack 相关命令如下：
  npm config set prefix "XXXX\nodejs\node_global"
  npm config set cache"XXXX\nodejs\node_cache"
  npm install express -g
  npm install -g cnpm --registry=https://registry.npm.taobao.org
  npm install --global vue-cli
  npm install -g webpack
```
### 初始化工程

```
npm install （如果速度问题可以使用cnpm）
```
### 启动

```
npm run serve
```

<<<<<<< HEAD
## 项目目录说明

<pre><code>
├──./                             <=vue前端项目工程
├──README.MD  
├── document                      <=一些遇见的问题的归档 
├── src                           <=项目源代码  
│ ├── views 
│ │  ├── content                  <= 内容页面 ，我习惯将所有的view跳转到的主要的view视图页面放置于此 
│ │  ├── footer                   <= footer 放置于此处
│ │  ├── header                   <= header 放置于此处
│ │  ├── x member x               <= 我之前是将全局的组件放置在members中，本项目不再使用此种方式  
├── components                     <= 将全局的组件放在此处
│ ├── xx_a                         <= xx_a 视图组件
│ │ ├── base                       <= xx_a 的基础组件
│ │ ├── bar                        <= xx_a 的bar组件
│ ├── common                       <= 一些公共的组件
├── router                         <= vue的router文件在此
├── store                          <= vuex 的所在路径
├── App.vue                        <= vue的起始视图页面，在 main.ts文件中引用 
├── main.ts                        <= vue的入口文件，我们使用了ts，所以为.ts 
</code></pre>

还有一些额外的说明

```
├── App.vue             <= 以前是将header与footer直接写在此处，由于此项目的实际home页面并没有header与footer，所以此处只有一个路由视图
│ ├── views
│ │  ├── Home.vue       <=  为不含header与footer的首页
│ │  ├── Content.vue    <=  保留header与footer组件的基础页面，其中会嵌套子路由
```

### 技术路线说明

vue + vuex + vue-router+webpack+es6+less

- 新加入了打包下载功能，主要包含打包下载页面的页面有 `data` 与 `product`  
  目前考虑的实现方式：  
  1- 先下载文件  
  2- zip压缩  
  3- 生成最终的文件    
  
[参考的文章](https://www.mmxiaowu.com/article/59b23f5d5b06a403cf687ed6)


### 引用的一些组件

使用了`vue-property-decorator`
=======
## 1. 工程目录结构
![](overview_image/1.png)

## 2. 目录说明
```
2.1、build：构建脚本目录

　　　　1）build.js   ==>  生产环境构建脚本；

　　　　2）check-versions.js   ==>  检查npm，node.js版本；

　　　　3）utils.js   ==>  构建相关工具方法；

　　　　4）vue-loader.conf.js   ==>  配置了css加载器以及编译css之后自动添加前缀；

　　　　5）webpack.base.conf.js   ==>  webpack基本配置；

　　　　6）webpack.dev.conf.js   ==>  webpack开发环境配置；

　　　　7）webpack.prod.conf.js   ==>  webpack生产环境配置；

　　2.2、config：项目配置

　　　　1）dev.env.js   ==>  开发环境变量；

　　　　2）index.js   ==>  项目配置文件；

　　　　3）prod.env.js   ==>  生产环境变量；

　　2.3、node_modules：npm 加载的项目依赖模块

　　2.4、src：这里是我们要开发的目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：

　　　　1）assets：资源目录，放置一些图片或者公共js、公共css。这里的资源会被webpack构建；

　　　　2）components：组件目录，我们写的组件就放在这个目录里面；

　　　　3）router：前端路由，我们需要配置的路由路径写在index.js里面；

　　　　4）App.vue：根组件；

　　　　5）main.js：入口js文件；

　　2.5、static：静态资源目录，如图片、字体等。不会被webpack构建

　　2.6、index.html：首页入口文件，可以添加一些 meta 信息等

　　2.7、package.json：npm包配置文件，定义了项目的npm脚本，依赖包等信息

　　2.8、README.md：项目的说明文档，markdown 格式

　　2.9、.xxxx文件：这些是一些配置文件，包括语法配置，git配置等
  ```
>>>>>>> 9815346834b6baceb8493d2009afa295f37a59cb
