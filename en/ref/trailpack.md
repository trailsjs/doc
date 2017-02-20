#### [Docs](../) / [API Reference](./) / Trailpack

# 9.2 Trailpack API

## `class` Trailpack

This interface is implemented by all Trailpacks, and is relied upon by the Trails Application to harmoniously load and manage them.

### `constructor (app, pack)`

Initialize the application with the provided Trails `app` instance.

| @param | type | description | required? |
|:---|:---|:---|:---|
| `app` | `Trails` | the Trails application instance | yes |
| `pack` | `Object` | the Trailpack definition | yes |
| `pack.pkg` | `Object` | the `package.json` of the Trailpack | yes. throws `PackageNotDefinedError` |
| `pack.config` | `Object` | any custom configuration that the Trailpack requires | no |
| `pack.api` | `Object` | any additonal API resources to mix-in to the Trails `app` | no |

#### How to Subclass

Each Trailpack subclass will provide the `pack` definition to this superclass. For example:

```js
module.exports = class BootstrapTrailpack extends Trailpack {

  /**
   * Trails passes in "app" as a reference to itself
   * @override
   */
  constructor (app) {

    // This constructor passes along the "app" reference, and provides its own custom
    // "pack" definition to the Trailpack superclass constructor.
    super(app, {
      pkg: require('./package'),
      config: require('./config'),
      api: { }
    })
  }
}
```

#### Instantiation

Trailpacks are never instantiated directly by the developer. Each Trailpack is instantiated in the [`Trails`](../trails.md) constructor like so:
```js
this.config.get('main.packs').forEach(Pack => new Pack(this))
```
where `this` references the `Trails` instance. Note that `pack` argument is required here because the Trailpack subclass will provide this to its superclass (this class).

## Methods

The following methods correspond to each of the four [Trailpack Lifecycle](./build/trailpack.md) stages:
- validate
- configure
- initialize
- unload

Note: Do not invoke these methods directly in your application code.

## `validate ()`

Validate any necessary preconditions for this trailpack. We strongly recommend that all Trailpacks override this method and use it to check preconditions.

```js
module.exports = class BootstrapTrailpack extends Trailpack {

  /**
   * Make sure a bootstrap method is available
   */
  validate () {
    if (!this.app.config.get('bootstrap')) {
      throw new Error('config.bootstrap is required!')
    }
  }
}
```

## `configure ()`

Set any configuration required before the trailpacks are initialized. Trailpacks that require configuration, or need to alter/extend the app's configuration, should override this method. After all Trailpacks have completed the `configure` phase (specifically, on the event `trailpack:all:configured`), the app configuration will be frozen. This is the last stage at which modifying app configuration is possible.

```js
module.exports = class BootstrapTrailpack extends Trailpack {

  /**
   * Bind the Trails app to the bootstrap function's context so that it can fire events.
   */
  configure () {
    this.config.bootstrap = this.config.bootstrap.bind(this.app)
  }
}
```

## `initialize ()`

This is the final stage in the Trailpack boot process. After the successful completion of the `validate` and `configure` stages, this method can now rely on having the necessary preconditions. In this stage, the Trailpack can start any necessary services or listeners that are needed to extend the framework functionality: start web servers, connect to databases, etc. **In short, this is the place to do whatever it is that your Trailpack is designed to do.**


```js
module.exports = class BootstrapTrailpack extends Trailpack {

  /**
   * Invoke the user-configured bootstrap function
   */
  initialize () {
    return this.config.bootstrap()
  }
}
```

## `unload ()`

This method is invoked when Trails is shutting down, either due to an unrecoverable runtime error, or because the user has explicitly invoked `.stop()`. This method should instruct the trailpack to perform any necessary cleanup with the expectation that the app will stop or reload soon thereafter. If your trailpack runs a daemon or any other thing that may occupy the event loop, implementing this method is important for Trails to exit correctly.

## Next: [Controller API](controller.md)
