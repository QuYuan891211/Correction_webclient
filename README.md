# 一. neargoos 前端工程

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



# 二. 前端部署

## 1. 安装Nginx依赖
```
安装Nginx所需的环境
	Ubuntu可以通过apt源安装以下依赖库，CentOS可以通过yum安装
	1.Nginx是C语言开发，需要gcc依赖库
	先检查本机是否有gcc环境
	gcc -v
	 如果没有gcc环境，则需要安装
	apt install gcc
	2.安装pcre依赖库
	PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。
	apt-get install libpcre3 libpcre3-dev
	3.安装zlib依赖库
	zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip 。
	apt-get install zlib1g-dev
	4.安装ssl依赖库
	OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
	nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要安装 OpenSSL 库。
apt-get install openssl
```
## 2. 安装Nginx
```
#apt-get update
#apt-get nginx
在nginx目录下启动
#nginx
```

## 3. 工程打包，放到服务器目录
```
#npm run build
```

## 4.修改nginx的配置文件，并重新启动nginx
```
#nginx -s reload
```
## 5.效果
