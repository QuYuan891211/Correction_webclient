# 预警报订正系统 前端工程

## 1. 安装
```
1.控制台默认目录下依次安装nodejs，npm(新版nodejs自带)，cnpm, vue-cil：
　　1.1 nodejs下载地址为：https://nodejs.org/en/
　　1.2 检查是否安装成功：分别输入node -v，npm -v,  如果输出版本号，说明我们安装node环境成功
    1.3 安装cnpm,命令如下: npm install -g cnpm --registry=https://registry.npm.taobao.org
        输入cnpm -v检查是否安装成功了
    1.4 安装vue脚手架工程，命令如下：npm install --global vue-cli
    1.5 安装webpack,命令如下：npm install -g webpack
```
### 2. 初始化工程
```
在VSCODE中打开correction-webclient文件夹，然后在菜单栏选择Terminal，在终端中输入以下命令：
npm install （如果速度问题可以使用cnpm）
```
### 3. 启动
```
在终端中输入以下命令：
npm run serve
如在浏览器出现VUE的log及Welcome to Your Vue.js App的字样 则说明成功
```
