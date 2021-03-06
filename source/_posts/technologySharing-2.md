---
title: 前端项目流程
date: 2022-04-23 18:59:38
tags:
---
# 前端项目篇

当我们要熟悉一个项目的时候，首先看其`package.json`的`scripts`属性下的执行命令有哪些? 其次看打包构建工具的配置文件如何写，如`webpack`的`config`。

## 1. 进行查找入口在哪里？

   根据其步骤，在启动命令`start`里的配置可以知道涉及到了两个配置文件，一个是`webpack.pub.config.js`，一个是`vite`其对应的就是`vite.config.js`。
<!--more-->
   ![package-scripts](technologySharing-2/project/package-scripts.png)

   进入`webpack.pub.config.js`先寻找`pub.js`的入口在哪里

   ![entry-webpack](technologySharing-2/project/entry-webpack.png)

## 2. 根据路径`./src/main/index.js`进行探究

   (1) 在`index.js`文件里确定了这个是入口，可以看出该文件是一个立即执行的匿名函数，异步调用了`init`目录下的`index.js`文件。

   ![index-init](technologySharing-2/project/index-init.png)

   (2) 该初始化文件先进行挂载兼容Window代码

   ![win-pub](technologySharing-2/project/win-pub.png)

   (3) 接着进行一系列的加载静态资源（Less Css）以及第三方插件（jq/jqtouch）,进行初始化配置文件，然后也挂载一下jq

   ![less-css-jq-mount](technologySharing-2/project/less-css-jq-mount.png)

   (4) 然后进行获取页面容器并切换容器，还有是否创建混入`Vue`的页面，这个还需要查看当前目录下的`hybrid.js`进行的配置的`VueActions`。

   ![Container-init](technologySharing-2/project/Container-init.png)

   ![vue-hybrid](technologySharing-2/project/vue-hybrid.png)

   (5) 进入`hybrid.js`文件进行探究时，可以发现原来`Vue的页面`是通过`iframe`进行嵌入到`pub.js`的页面里的。以此，我们可以知道：**pubjs包含Vue的集合关系**。

   ![iframe-vue](technologySharing-2/project/iframe-vue.png)

   (6) 我们继续返回研究`init/index.js`文件还有哪些初始化，这里我们看出还进行了`获取用户登录状态`然后进行`路由的鉴权`

   ![router-check](technologySharing-2/project/router-check.png)

   （7）再下来就是对`pubjs`的`plugins`下的一些模块进行引入，然后进行路由的初始化。这里面近期还引入了一个新的打包优化，稍等根据两个新文件进行分析其作用。

   ![init-plugins-router-components](technologySharing-2/project/init-plugins-router-components.png)

   （8）在这里的新增的`InitComponents()`是进行高频组件的注册，而导入的`router.js`这个文件是`webpack`打包的时候写入的，其里面的注册方式比较不一样，采用`动态import导入`的。

   `webpack.pub.config.js`配置文件：

   ![webpack-rounter-write](technologySharing-2/project/webpack-rounter-write.png)

   ![webpack-router-create](technologySharing-2/project/webpack-router-create.png)

   根据目标写入后生成的`router.js`的内容：

   ![router-import](technologySharing-2/project/router-import.png)

   （9）在这个`动态import导入`的好处是可以进行`分包`进行`懒加载`，从而可以提高**首屏的渲染速度**。在`webpack.pub.config.js`配置文件还进行了分包配置使用了`splitChunks`,在webpack这一块，里面还使用了一些技巧,比如对`代码的压缩`，还有开发时开启`devServer`的时候把写入`内存`的改为写入`磁盘`。这样子就不怕内存少跑不了多个项目啦

   ![webpack-splitChunks](technologySharing-2/project/webpack-splitChunks.png)

   (10) 我们继续回归主题，探究初始化还有哪些操作？接下来进行了发送登录请求，登录成功后，记录登陆地址，后面还进行了对登录状态一个`轮询`。也还进行了一些`自定义权限`检查。

   ![init-login](technologySharing-2/project/init-login.png)

   (11) 最后一步就进行一些路由的挂载，然后启动路由。

   

## 总结下来的流程图如下：

   ![init-step](technologySharing-2/project/init-step.png)


## 业务流程

   ![business-flow](technologySharing-2/project/business-flow.png)

## 项目部署流程


![jenkins-flow](technologySharing-2/project/jenkins-flow.png)

拆开细一点的图
![qa-CICD](technologySharing-2/project/qa-CICD.png)


## 总结

(1) 前端的项目是`pub.js`嵌入`iframe`引入`Vue`的页面，但是这种做法有一些缺点：
1. 只能在浏览器中使用 `iframe`；
2. 需要向 `DOM` 添加一个 `iframe` 以对其进行初始化；
3. 每个 `iframe` 环境都包含完整的 `DOM`，这在一些场景下限制了自定义的灵活度；
4. 默认情况下，`iframe`对象是可以跨环境的，这意味着需要额外的工作来确保代码安全。

目前有一个实验性属性`CSP`（内容安全策略）。内容安全策略(`CSP`)是一个额外的安全层，用于检测并削弱某些特定类型的攻击，包括跨站脚本 (`XSS`) 和数据注入攻击等。


(2) 可以采用一个新的`JavaScript`提案：`ShadowRealm API`。`ShadowRealm`目前已经进入`stage-3`阶段。进入这个阶段的提案也就是意味着基本稳定。`ShadowRealm API`的优势：
1. `ShadowRealm`允许一个JS运行时创建多个高度隔离的JS运行环境（`realm`），每个`realm`具有独立的全局对象和内建对象。
2. 服务器可以在 `ShadowRealm` 中运行第三方代码。
3. 在 `ShadowRealm` 中可以运行测试，这样外部的JS执行环境不会受到影响，并且每个套件都可以在新环境中启动（这有助于提高可复用性）
4. 网页抓取(爬虫，从网页中提取数据)也可以在 `ShadowRealm` 中运行。
   
在`ShadowRealm`中运行Web应用，`jsdom` 库创建了一个封装的浏览器环境，可以用来测试 `Web` 应用、从 `HTML` 中提取数据等。它目前使用的是 `Node.js vm` 模块【`Nodejs vm`模块它能控制import语法。控制了import，基本就就控制了整个隔离。】，未来可能会更新为使用 `ShadowRealm`（后者的好处是可以跨平台，而 `vm` 目前只支持 `Node.js`）。


[关于ShadowRealm参考信息](https://2ality.com/2022/04/shadow-realms.html)

