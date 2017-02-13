# Configuration

Trails provides a unified way to configure all facets of your application. All configuration files are located in the `config/` directory.

# Config Files

## [`index.js`](index.md)

A manifest of the available configuration files.

## [`main.js`](main.md)

Declare the Trailpacks to load, set filesystem paths, and other basic application settings.

## [`log.js`](log.md)

Setup the logger that Trails will use during runtime.

## [`routes.js`](routes.md)

Define how web requests are routed to controllers.

## [`stores.js`](stores.md)

Define data stores. Each [Model](../concepts/Model) maps itself to a "store".

## [`i18n.js`](i18n.md)

Setup internationalization, define multi-language mappings, etc.

## [`web.js`](web.md)

Configure the web server. Set the listening port, load any plugins, and define advanced web server settings.

# Environment

Trails can load environment-dependent configuration based on the `NODE_ENV` environment variable. By default, `NODE_ENV=development`. By creating separate configurations inside `config/env/`, different configurations are loaded based on the value of `NODE_ENV`. Note: Anytime you add a config file, remember to add it to the [`index.js`](index.md) manifest!

## Example

When `NODE_ENV=production`, the following configuration in `config/env/production.js` will override the existing value of `config.web.hostname` set in `config/web.js`.

```js
// config/env/production.js
module.exports = {
  web: {
    hostname: 'trailsjs.io'
  }
}
```

# Runtime Configuration

## Access

The application configuration is available at `this.config` inside all Controllers, Services, and Policies. The nested objects in the configuration files are flattened into a [`Map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map).

```js
// api/services/MapService.js
module.export = class MapService extends Service {

  /**
   * Renders a single map tile
   */
  getTile ({ x, y }) {
    const threads = this.config.get('map.renderThreads')

    return this.getMap({ x, y }, threads)
  }
}
```

## Modify

The configuration is frozen and *immutable* during runtime; specifically, the configuration is frozen after all Trailpacks complete their `configure` stage.

```js
// trailpack-mapnik/index.js
module.exports = class MapnikTrailpack extends Trailpack {

  /**
   * Set the default number of renderThreads based on the number of CPU cores
   * available on the system.
   */
  configure () {
    const cpuCores = os.cpus().length
    this.config.set('map.renderThreads', cpuCores)
  }

  /**
   * Changed my mind; need to set map.renderThreads to something else
   */
  initialize () {
    this.config.set('map.renderThreads', 4) // throws IllegalAccessError
  }
}
```
