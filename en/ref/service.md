#### [Docs](../) / [API Reference](./) / Service

##### *If you're looking for the walkthrough guide on Services, see: [2.2 Service](../build/service.md)*

# 9.5 Service API

## `class` Service

Trails Services contain most of the application's business logic. Service methods are invoked by Controller handlers to process requests.
Service classes are singletons; that is, each subclassed Service is instantiated once, and that reference is maintained during the life of the program.

### `constructor (app)`

Initialize the Service with the provided `app` instance.

| @param | type | description | required? |
|:---|:---|:---|:---|
| `app` | `Trails` | the Trails application instance | yes |

## Methods

None.

## Fields

### `id`

Return the id of the Service. e.g. for `UserService`, the `id` will be `user`.

### `log`

The application's Logger instance. Convenience reference for `this.app.log`.

### `__`

The i18n translator instance. Convenience reference for `this.app.__`.

### `services`

Convenience reference to `this.app.services`.

### `models`

Convenience reference to `this.app.models`.

## Next: [Model API](model.md)
