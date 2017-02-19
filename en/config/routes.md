#### [Docs](../../) / [Configuration](./) / routes.js

# 3.4. `routes.js`

Define how web requests are routed to controllers. The Route objects are modeled on the hapi.js [Route Specification](https://hapijs.com/tutorials/routing). Some plugins such as [trailpack-footprints](https://github.com/trailsjs/trailpack-footprints) may auto-generate Route configuration based on other facets of the application.

```js
module.exports = [

 /**
  * GET requests to /default/info will invoke DefaultController.info
  */
  {
    method: [ 'GET' ],
    path: '/default/info',
    handler: 'DefaultController.info'
  }
]
```

## `handler`

A string that defines the [Trails Controller](../build/controller.md) method that will handle the request.

## `method`

The Route `method` can be the string of a single http method (e.g. `POST`) or an array of the http methods that will be accepted for this route.

## `path`

The route will handle requests to URLs that match `path`.

### Path Paramters

Path parameters can be defined, and will be passed into their handlers. [Full Specification](https://hapijs.com/api#path-parameters).

```js
// config/routes.js
module.exports = [
  {
    method: [ 'GET' ],
    path: '/map/tile/{x}/{y}/{z?}',
    handler: 'MapController.getTile'
  }
]
```

```js
// api/controllers/MapController.js
module.exports = class MapController extends Controller {

  /**
   * @param request.params.x  The x coordinate of the map tile
   * @param.request.params.y  The y coordinate of the map tile
   * @param.request.params.z  The zoom level of the map (optional)
   */
  getTile (request, reply) {
    const { x, y, z } = request.params

    reply(MapService.getTile({ x, y, z }))
  }
}
```

## `config`

Each route can define other custom properties in a nested `config` object. [Full Specification](https://hapijs.com/api#route-options). Below are some common uses.

### Self-Documenting Routes

Some doc generation tools, such as Swagger, can use the `config` object to glean contextual information about the route. Full


```js
{
  method: [ 'GET' ],
  path: '/map/tile/{x}/{y}/{z?}',
  handler: 'MapController.getTile',
  config: {
    description: 'Render a map Tile',
    notes: 'The "z" param will default to config.map.defaultZoom if not given',
    tags: [ 'api', 'map', 'tile' ]
  }
}
```

### Access Control

Pre-requisites can be defined for a route, which are a list of [Trails Policies](../build/policy.md) that must pass before the route handler is invoked.

```js
{
  method: [ 'GET' ],
  path: '/map/tile/{x}/{y}/{z?}',
  handler: 'MapController.getTile',
  config: {
    /**
     * Ensure that the client has a valid API key for this Map tile request
     */
    pre: [
      'MapPolicy.verifyApiKey'
    ],

    /**
     * Only allow requests from *.trailsjs.io domains
     */
    cors: {
      origin: [
        '*.trailsjs.io'
      ]
    }
  }
}
```
