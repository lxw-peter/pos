# pos

> 演示地址：https://lxw-peter.github.io/pos/

### Project setup

```
npm install

npm run serve

npm run build

npm run test

npm run lint
```

### 项目截图：

![设计草图](https://ws1.sinaimg.cn/large/0061BkIsly1fwn4fuqsbpj30u70mdwfe.jpg)

![项目截图](https://ws1.sinaimg.cn/large/0061BkIsly1fwn47884owj312j0rq40u.jpg)

本项目使用的是vue-cli3 进行项目开发环境搭建，中间也踩了一些坑，后续会把这些坑都给记录在这里：

1. 如何发布以下面这种形式https://github.com/lxw-peter/pos发布到线上？

   跟目录新建一个vue.config.js文件，这个配置文件在vue-cli2.x版本是自带的，3之后的版本剔除了，只能手动配置

   ```js
   module.exports = {
       // 部署应用包时的基本 URL ，使用相对路径
       baseUrl:'/pos/'
   }
   ```


感谢：

​	这个项目是跟着技术胖的博客开发的，感谢！

原文链接：http://jspang.com/post/vuedemo.html

​	