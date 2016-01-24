# trailpack-footprints
###### Trails Core Component Documentation - footprints

[![Gitter][gitter-image]][gitter-url]
[![NPM version][npm-image]][npm-url]
[![Build status][ci-image]][ci-url]
[![Dependency Status][daviddm-image]][daviddm-url]
[![Code Climate][codeclimate-image]][codeclimate-url]

Footprints Trailpack. This trailpack provides the footprint interface, which
other trailpacks such as [trailpack-waterline](https://github.com/trailsjs/trailpack-waterline)
and [trailpack-knex](https://github.com/trailsjs/trailpack-knex) implement,
as well as a suite of tests that Footprint implementations should pass.

![Trails Footprints Diagram][diagram-image]

## What are Footprints?

Footprints automatically generate easy-to-use RESTful endpoints for your models.

## Install

```sh
$ npm install --save trailpack-footprints
```

## Configure

```js
// config/main.js
module.exports = {
  packs: [
    // ... other trailpacks
    require('trailpack-footprints')
  ]
}
```

```js
// config/footprints.js
module.exports = {
  /**
   * Generate routes for controller handlers.
   */
  controllers: true,

  /**
   * Generate conventional Create, Read, Update, and Delete (CRUD) routes for
   * each Model.
   */
  models: {
    options: {

      /**
       * The max number of objects to return by default. Can be overridden in
       * the request using the ?limit argument.
       */
      defaultLimit: 100,

      /**
       * Subscribe to changes on requested models via WebSocket
       * (support provided by trailpack-websocket)
       */
      watch: false,

      /**
       * Whether to populate all model associations by default (for "find")
       */
      populate: true
    },

    actions: {
      create: true,
      find: true,
      update: true,
      destroy: true,

      /**
       * Specify which "association" endpoints to activate.
       */
      createAssociation: true,
      findAssociation: true,
      updateAssociation: true,
      destroyAssociation: true
    }
  },

  /**
   * Prefix your footprint route paths
   */
  prefix: '/api/v1'
}
```

## API

### `api.services.FootprintService`

The purpose of `FootprintService` is to transform and forward queries to the datastore.

#### `create (modelName, values, [options])`

| param | required? | description | example |
|:---|:---|:---|:---|
| `modelName` | Yes | The name of the model to create (in `api.models`) | `User` |
| `values` | Yes | An object containing the values of the record to create | `{ username: 'admin' }` |
| `options` | No | Datastore-specific options |

#### `find (modelName, criteria, [options])`

| param | required? | description | example |
|:---|:---|:---|:---|
| `modelName` | Yes | The name of the model to search for (in `api.models`) | `User` |
| `criteria` | Yes | An object containing the query criteria | `{ username: 'admin' }` |
| `options` | No | Datastore-specific options |

#### `update (modelName, criteria, values, [options])`

| param | required? | description | example |
|:---|:---|:---|:---|
| `modelName` | Yes | The name of the model to create (in `api.models`) | `User` |
| `criteria` | Yes | An object containing the query criteria | `{ username: 'admin' }` |
| `values` | Yes | An object containing the values to update | `{ username: 'tjwebb' }` |
| `options` | No | Datastore-specific options |

#### `destroy (modelName, criteria, options)`

| param | required? | description | example |
|:---|:---|:---|:---|
| `modelName` | Yes | The name of the model to create (in `api.models`) | `User` |
| `criteria` | Yes | An object containing the query criteria | `{ username: 'admin' }` |
| `values` | Yes | An object containing the values to update | `{ username: 'tjwebb' }` |
| `options` | No | Datastore-specific options |

#### `createAssociation (parentModelName, parentId, childAttributeName, values, [options])`

#### `findAssociation (parentModelName, parentId, childAttributeName, criteria, [options])`

#### `updateAssociation (parentModelName, parentId, childAttributeName, criteria, values, [options])`

#### `destroyAssociation (parentModelName, parentId, childAttributeName, criteria, [options])`

### `api.controllers.FootprintController`

The purpose of the `FootprintController` is to transform and forward requests to the `FootprintService`.


[diagram-image]: http://i.imgur.com/olRxPS8.png
[npm-image]: https://img.shields.io/npm/v/trailpack-footprints.svg?style=flat-square
[npm-url]: https://npmjs.org/package/trailpack-footprints
[ci-image]: https://img.shields.io/travis/trailsjs/trailpack-footprints/master.svg?style=flat-square
[ci-url]: https://travis-ci.org/trailsjs/trailpack-footprints
[daviddm-image]: http://img.shields.io/david/trailsjs/trailpack-footprints.svg?style=flat-square
[daviddm-url]: https://david-dm.org/trailsjs/trailpack-footprints
[codeclimate-image]: https://img.shields.io/codeclimate/github/trailsjs/trailpack-footprints.svg?style=flat-square
[codeclimate-url]: https://codeclimate.com/github/trailsjs/trailpack-footprints
[gitter-image]: http://img.shields.io/badge/+%20GITTER-JOIN%20CHAT%20%E2%86%92-1DCE73.svg?style=flat-square
[gitter-url]: https://gitter.im/trailsjs/trails
