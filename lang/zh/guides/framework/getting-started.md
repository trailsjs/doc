# 入门指南
###### Trails - 用户指导

你将学习到Trails相关的基础知识，怎样创建一个Trails项目，和Trails项目的基础结构。

本文假定你的机器上已经安装了Node.js 4.0.0(或更高版本)。掌握一些MVC和JavaScript的知识是前提。

## Trails是什么 ?

Trails是一个新的用ES2015 (ES6)写的 Node.js MVC 框架. 
这是一个模块化的框架，允许你选择日常使用的框架，并使他们能很好地在一起工作。

## 安装

Trails 使用 `yeoman` 来生成基础项目. 如果你现在还没有安装它, 现在运行 :

`npm install -g yo`

接着安装 `yeoman`  Trails 的生成器 :

`npm install -g generator-trails`

你现在已经准备好创建第一个项目了 ! :)

## 创建项目

创建项目十分容易. 只要跟随本文的步骤 :

```
mkdir myproject;
cd myproject;
yo trails;
```

你将看到如下的一些信息 :

![.Web server choice](/assets/img/tutoYoTrails1.png)

第一步是选择web服务器框架. 默认是 hapi，你可以选择你更喜欢的另一个框架. 这个选项将决定你如何书写 `policies` 和 `controllers`, 这个我们一会儿再说.

接下来的步骤是 `package.json` 信息. 依次是 :

* 项目名称, 默认是所在文件夹的名字, 这里我们设置默认为 `myproject`
* 项目描述
* 项目主页链接地址, 你的项目仓库的url
* 作者名字, 你 :)
* 作者邮箱
* 作者主页
* 包的关键字
* GitHub用户名或组织名称
* 你的网站
* 许可证

在这之后，生成器会创建你的项目结构，并安装依赖 (`npm install`).

![.Project generated](/assets/img/tutoYoTrails2.png)

## 运行项目

就像yeoman生成器的引导一样，你只需要运行:

`node server.js`

你的服务器现在应该运行在3000端口，在你的console终端里你会看到:

![.Start log](/assets/img/tutoYoTrails3.png)

如果你打开 http://localhost:3000 你会看到:

![.Hello Trails.js !](/assets/img/tutoYoTrails4.png)

停止服务器， 连按`Ctrl+c`两次.

## 如何使用Yeoman生成器

Trails使用[Yeoman](http://yeoman.io/)来生成新项目的脚手架, 并创建应用内的资源.

```sh
$ yo trails --help

Usage:
  yo trails

Generators:

  Create New Model
    yo trails:model <model-name>

  Create New Controller
    yo trails:controller <controller-name>

  Create New Policy
    yo trails:policy <policy-name>

  Create New Service
    yo trails:service <service-name>
```

## Project structure
Now your server is running, let see how :)

![.Project structure](/assets/img/tutoYoTrails5.png)

### API
As I said earlier, Trails is a MVC framework. MVC stands for Model-View-Controller. This is a software design pattern is used to separate an application's concerns:

- Model : The Model represents an object or Javascript POJO (Plain Old JavaScript Object) carrying data. It can also have logic to update controller if its data changes.
- View : The View represents the visualization of the data that model contains.
- Controller : The Controller acts on both model and view. It controls the data flow into model objects and updates the view whenever data changes. It keeps view and model separate.

Here in Trails, the files structure is pretty common: you have a folder `api` for all your `controllers`, `models`, `policies` and `services`.

As you can see, the MVC design pattern in Trails is slightly different; it adds 2 new layers: `policies` that run before controllers, and `services` who make controllers methods more accessible.

Let's see how the default file structure looks.

#### Models
Model files depends on your ORM. By default Trails uses Waterline, so model definition will look like (http://waterline.js.org/docs/models/models.html) :

```
'use strict'
const Model = require('trails-model')
/**
 * User
 *
 * @description A User model
 */
module.exports = class User extends Model {
  static schema() {
    return {
      username: 'string'
    }
  }
}
```
Waterline will take care of database/table creation for you, by default.

You can create a new model with this command :
`yo trails:model myModelName`

WARNING: if you don't use `yo` to generate your models don't forget to add them under `api/models/index.js` or it will be ignored.

#### Controllers
If you remember, when setting up our Trails app, we chose hapi as our web server, so all controller functions have to look like hapi middleware functions.
Here is what you will have in `ViewController.js` :
```
'use strict'
/**
 * @module ViewController
 *
 * @description Default Controller included with a new Trails app
 * @see {@link http://trailsjs.io/doc/api/controllers}
 * @this TrailsApp
 */
module.exports = class ViewController {
	/**
   *
   */
  helloWorld (request, reply) {
    reply('Hello Trails.js !')
  }
}
```
Here is what you should see when you open your browser on http://localhost:3000. But how is this function mapped to route `/` ? You will see this in Config section.

If your controller(s) extends 'trails-controller' then your controller(s) methods must have to implement only hapi interface (with `request` and `reply`) and your controllers will work with hapi, express4 or any webserver trailpack who support standard Trails controllers.
If your controllers doesn't extends this class like the example above, your controller methods can implement native interface of the choosen webserver (`request`, `reply` for hapi, `req`, `res`, `next` for express4...).

You can create new controllers with this command :
`yo trails:controller myControllerName`

WARNING: if you don't use `yo` to generate your controllers don't forget to add them under `api/controllers/index.js` or it will be ignored.

#### Services
Services are basically the brains of your project, they look like (`DefaultService.js`) :
```
'use strict'
const _ = require('lodash')
const Service = require('trails-service')

/**
 * @module DefaultService
 *
 * @description Default Service included with a new Trails app
 * @see {@link http://trailsjs.io/doc/api/services}
 * @this TrailsApp
 */
module.exports = class DefaultService extends Service {

  /**
   * Return some info about this application
   */
  getApplicationInfo () {
    return {
      app: this.pkg.version,
      node: process.version,
      libs: process.versions,
      trailpacks: _.map(_.omit(this.packs, 'inspect'), pack #> {
        return {
          name: pack.name,
          version: pack.pkg.version
        }
      })
    }
  }
}
```
And the service can be called from any controller like this : `this.app.services.DefaultService.getApplicationInfo()` (see DefaultController.js)

You can create new services with this command :
`yo trails:service myServiceName`

WARNING: if you don't use `yo` to generate your services don't forget to add them under `api/services/index.js` or it will be ignored.

#### Policies
Policies are functions that run before the controllers to check and validate rules, data, or whatever you need.
Like controllers, if you have chosen hapi a policy file should look like :
```
'use strict'

/**
 * @module Default
 * @description Test document Policy
 */
module.exports = class Default {
  info(request, reply) {
    reply()
  }
}
```
But how is this function mapped to a controller? You will see this in Config section.

You can create new policies with this command :
`yo trails:policy myPolicyName`

WARNING: if you don't use `yo` to generate your policies don't forget to add them under `api/policies/index.js` or it will be ignored.

### Config
The `config` folder contains all the config file for each part of the server. You can change the running port under `config/web.js`, or the template engine in `config/views.js`.

- env : Config file that overrides the default per environment, you can change the entire config for your production environment
- database : Information about your database: type, migration strategy, et cetera.
- footprints : Options for your REST API: add a prefix, enable/disable footprint, et cetera.
- i18n  : Configuration for multi language support: set default language and translations.
- locales : Folder in which to put all translation files
- log : Logger options for your project. Uses winston by default.
- main : Allow you to enable/disable trailpacks.*
- policies : Allow you to map your policies with controllers.
- routes : Allows you to map your controller's functions with routes.
- session : Options for managing sessions: strategies, cookies, web tokens, et cetera.
- views : Allow you to configure your template engine.
- web : Options for web server, like port.
- webpack : Configuration for building assets, it's a default webpack configuration (http://webpack.github.io/docs/configuration.html)

NOTE: `trailpacks*`: All modules used by Trails are call Trailpack. For example to use Hapi as web server, Trails uses a Trailpack named `trailpack-hapi`. Don't hesitate to look on npm to find some cool Trailpacks.

WARNING: if you add some config files don't forget to add them under `config/index.js` or it will be ignored.

You now have the basic knowledge to make a simple Trails project!

## More ?
Did you notice that when you start your project you have `trails>`?

Try to write `app`, `app.api.controllers`, `app.api.services` or even `get('/api/v1/default/info')`. Cool isn't it, this is one feature of trailpack-repl package, that implement the possibility to debug from interactive shell.

### LICENSE

[MIT](LICENSE)
created by @jaumard
