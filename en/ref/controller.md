#### [Docs](../) / [API Reference](./) / Controller

##### *If you're looking for the walkthrough guide on Controllers, see: [2.1 Controller](../build/controller.md)*

# 9.3 Controller API

## `class` Controller

Trails Controller classes are singletons; that is, each subclassed Controller is instantiated once, and that reference is maintained during the life of the program.

### `constructor (app)`

Initialize the Controller with the provided `app` instance.

| @param | type | description | required? |
|:---|:---|:---|:---|
| `app` | `Trails` | the Trails application instance | yes |

## Methods

None.

## Fields

### `id`

Return the id of the Controller. e.g. for `UserController`, the `id` will be `user`.

### `log`

The application's Logger instance. Convenience reference for `this.app.log`.

### `__`

The i18n translator instance. Convenience reference for `this.app.__`.

### `services`

Convenience reference to `this.app.services`.

## Next: [Policy API](policy.md)
