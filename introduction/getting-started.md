# Getting Started

The Getting page covers how to start a Trails app from scratch.

## First Step : Installation
Please follow the [installation Guide](installation.md).

## Second Step : Use Generators

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

## Third Step : Run

Once installation is complete, begin your journey!


```sh
$ node server.js
```

We don't recommend the use of npm start because of memory wastes, but you can still use it.

For more information about npm start memory wastes, please read:
[The “npm start” pattern wastes a lot of RAM](https://medium.com/@tjwebb/the-npm-start-default-uses-a-lot-of-ram-3e0d8ac0c6a1#.15akp5wmc)
