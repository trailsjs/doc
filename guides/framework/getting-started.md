# Getting Started
###### Trails - UserGuides

You will learn some basic information about Trails, how to create a Trails project, and the basic structure of a Trails project.

This guide assumes that you have previously installed Node.js 4.0.0 (or higher) on your machine. Some knowledge of MVC and Javascript is needed.

## What is Trails ?

Trails is a new Node.js MVC framework write in ES2015 (ES6). It's a modular framework which allows you to choose your every day framework(s) and make them work together.

## Installation

Trails use `yeoman` to generate a basic project. So if you haven't installed it yet, do it now :

`npm install -g yo`

Now install the `yeoman` generator for Trails :

`npm install -g generator-trails`

You are now ready to create your first application ! :)

## Create a project

Creating a project is very easy. Just follow this guide :

```
mkdir myproject;
cd myproject;
yo trails;
```

You should see something like this :

![.Web server choice](/assets/img/tutoYoTrails1.png)

The first step is to choose the web server. By default it's hapi but you can choose another one if prefer. This choice will affect how you write your `policies` and `controllers`, but we will se this later.

The other steps are for `package.json` information. In order :

* Name of your project, by default the name of the folder, here we set the default to `myproject`
* Description of your project
* Project homepage url, url of your project repository
* Author's Name, you :)
* Author's Email
* Author's Homepage
* Package keywords
* GitHub username or organization
* Your website
* License

After this the generator will create your project structure and install dependancies (`npm install`).

![.Project generated](/assets/img/tutoYoTrails2.png)

## Run a project

Like the yeoman generator instructs, you only need to do :

`npm start`

Your server should now be running on port 3000, and you should see on your console :

![.Start log](/assets/img/tutoYoTrails3.png)

And if your open http://localhost:3000 you should see :

![.Hello Trails.js !](/assets/img/tutoYoTrails4.png)

To stop server you need to `Ctrl+c` twice.

## How to use Yeoman Generators

Trails uses [Yeoman](http://yeoman.io/) to generate scaffolding for new
applications, and to create resources inside the application.

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
