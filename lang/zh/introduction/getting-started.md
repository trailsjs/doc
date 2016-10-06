# 新手指南

新手指南包含如何从头开始一个Trails应用.<br>
更多详细的新手教程请参考 : [详细说明](/guides/framework/getting-started.md)

## 第一步 : 安装
请参考[安装指南](installation.md).

## 第二步 : 使用脚手架生成器

Trails使用[Yeoman](http://yeoman.io/)来生成新应用的脚手架，创建应用内需要的资源。

```sh
$ yo trails --help

用法:
  yo trails

生成器:

  创建新的Model
    yo trails:model <model-name>

  创建新的Controller
    yo trails:controller <controller-name>

  创建新的Policy
    yo trails:policy <policy-name>

  创建新的Service
    yo trails:service <service-name>
```

## 第三步 : 运行

一旦安装就绪，开始你的旅程吧！


```sh
$ node server.js
```

我们不推荐使用npm start，因为会有内存浪费，但你仍可以使用它。

想了解更多关于npm start内存浪费的信息，请阅读：
[The “npm start” pattern wastes a lot of RAM](https://medium.com/@tjwebb/the-npm-start-default-uses-a-lot-of-ram-3e0d8ac0c6a1#.15akp5wmc)
