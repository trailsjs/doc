# How to create a Trailpack
###### Trails - UserGuides

## Introduction

The Getting page covers how to start a Trails app from scratch.

Hi guys ! Here you will learn how to create a customized Trailpack for Trails. You will see project's structure and how to run and test it before publishing on npm. Hope you're ready !

NOTE: This tutorial assumes that you have previously installed Node.js 4.0.0 (or more) on your machine, also have a Trails project created (see [Getting started with trails](http://blog.jaumard.com/en/2016/01/05/getting-started-with-trails)).

## What is a Trailpack ?

If you followed my first tutorial carefully, you already know this ! But I do not blame you if you forgot ^^, a Trailpack is a simple module which extends Trails functionalities. It can be a web server like express4, a tasks runner like grunt...

## Installation
Trailpack can be generated with yeoman. So if you didn’t install it, you should do it now :

`npm install -g yo`

Now, you can install the yeoman generator for Trailpack :

`npm install -g generator-trailpack`

You are finally ready to create your first trailpack ! :)

## Create a project
That’s very easy, just follow the guide :

```
mkdir myTrailpack;
cd myTrailpack;
yo trailpack;
```

Steps to personalized your trailpack are in following order:

. Name of your project (by default, it’s the name of the folder), here we let default’s name `myTrailpack`
. Description of your project
. Project’s homepage url: url of your project repository
. Author's Name, generally, you :)
. Author's Email
. Author's Homepage
. Package keywords
. GitHub username or organization
. Your website
. License

After this, the generator will create your project structure and install dependancies (`npm install`).

![Project generated](/assets/img/tutoYoTrailpack1.png)

## Project structure
You should see something like this :

![Project structure](/assets/img/tutoYoTrailpack2.png)

### API
As with Trails, you can generate api files with yo  :

```
yo trailpack:api apiName;
yo trailpack:model modelName;
yo trailpack:controller controllerName;
yo trailpack:policy policyName;
yo trailpack:service serviceName;
```

WARNING: if you don't use `yo` to generate API, don't forget to add those into `index.js` as in following: if it’s a model, add it in `models/index.js`, if it’s a controller add it in `controllers/index.js`... If your file is not in `index.js`, it will be ignored !

WARNING 2:
If your Trailpacks need Models, Controllers and Policies so your class must implement `trails-model`, `trails-controller` and `trails-policy` in order to work with more than one orm/webserver. Those class need to implement Waterline interface definition (models) and Hapi (controllers, policies).

### Configuration
You can put your own file’s configuration here. As you can see, a `trailpack.js` file communicates Trails you provide some APIs and configuration to the project (`provides` field). But it allows you too to configure the lifcycle of your trailpack. Indeed, if your trailpack needs to wait for some events or emit ones, you can put those here :

```
/**
 * Trailpack Configuration
 *
 * @see {@link http://trailsjs.io/doc/trailpack/config
 */
module.exports = {

  /**
   * API and config resources provided by this Trailpack.
   */
  provides: {
    api: {
      controllers: [ ]
      // ...
    },
    config: [ ]
  },

  /**
   * Configure the lifecycle of this pack; as how it boots up, and in which
   * order it loads relatively to other trailpacks.
   */
  lifecycle: {
    configure: {
      /**
       * List of events which must be fired before the configured lifecycle
       * method is invoked in this Trailpack
       */
      listen: [ ],

      /**
       * List of events emitted by the configured lifecycle method
       */
      emit: [ ]
    },
    initialize: {
      listen: [ ],
      emit: [ ]
    }
  }
}
```

Let see how our Trailpack can interact with Trails server.

### Trailpack class
It is your Trailpack’s core. It’s this file which allows you to interact with Trails server. This class is under `index.js` and looks like :
```
const Trailpack = require('trailpack')

module.exports = class Archetype extends Trailpack {

  /**
   * TODO document method
   */
  validate () {

  }

  /**
   * TODO document method
   */
  configure () {

  }

  /**
   * TODO document method
   */
  initialize () {

  }

  constructor (app) {
    super(app, {
      config: require('./config'),
      api: require('./api'),
      pkg: require('./package')
    })
  }
}
```
First, you want to rename `Archetype` by your Trailspack’s name (here `MyTrailpack`). As you can see, you have three methods in it :

- validate
- configure
- initialize

WARNING : if you want to create a Web Server or ORM compatible Trailpack you need to use `trailpack-webserver` / `trailpack-datastore` instead of `trailpack`. For more information see those repo : [trailpack-webserver](https://github.com/trailsjs/trailpack-webserver) and [trailpack-datastore](https://github.com/trailsjs/trailpack-datastore)

#### Validate
Validate function is the first called. As it’s named, you can check if the configuration of your trailpack is correct (if it’s needed). For example, the hapi trailpack checks if web server configuration under `config/web.js` is correct. In this tutorial we'll just add some log.

```
validate () {
	this.app.config.log.logger.info('My Trailpack is valid')
	return Promise.resolve()
}
```
At last, we call `Promise.resolve()` to tell Trails our Trailpack is valid, if it's not we can call `Promise.reject()`

#### Configure
Next come the configure function, this method is called after all events waiting in your Trailpack (see Configuration section below).
You can configure your Trailpack removing the hapi Trailpack. Here, it’s created the web server and configured all routes bind controllers and policies...

```
configure () {
	this.app.log.info('My Trailpack is configured')
	return Promise.resolve()
}
```
At last, we call `Promise.resolve()` to tell Trails our Trailpack is valid, if it's not we can call `Promise.reject()`

#### Initialize
Initialize is the final method called. Here, you must start your Trailpack (to do the job it was first here for ^^), hapi launches the web server on host and port configured on `configure` function.

```
initialize () {
	this.app.log.info('My Trailpack is initialized')
	return Promise.resolve()
}
```
At last, we call `Promise.resolve()` to tell Trails our Trailpack is valid, if it's not we can call `Promise.reject()`

We understood how to interact with a Trails server, let see now how to run and test our new Trailpack.

## Run and test
We wrote our Trailpack and that's nice but... How can I check if it's working !?

To reach this goal, you’ll need a Trails project and some npm commands :)

First, link your entire Trailpack with this command :

```
cd myTrailpack;
npm link;
```

Then, go to your Trails project and install your Trailpack :
```
cd myTrails;
npm link myTrailpack;
```

All you need to do now, in order to run it, is to edit `config/main.js` your Trails project and add your trailpack in `packs` :

```
/**
 * Trailpack Configuration
 * (app.config.main)
 *
 * @see http://trailsjs.io/doc/config/main
 */
module.exports = {

  /**
   * Order does *not* matter. Each module is loaded according to its own
   * requirements.
   */
  packs: [
    require('trailpack-core'),
    require('trailpack-repl'),
    require('trailpack-router'),
    require('trailpack-hapi'),
    require('myTrailpack')
  ]
}

```

Start your server with `npm start` and you should see if your trailpack is executed :

![Trailpack logs](/assets/img/tutoYoTrailpack3.png)

Now you are ready to create some new cool Trailpacks! Make sure to check NPM before you begin writing, there may already be a trailpack that does exactly what you want ;)

See you guys later !

### LICENSE

[MIT](LICENSE)
created by @jaumard
