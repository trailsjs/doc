#### [Docs](../) / [API Reference](./) / Policy

##### *If you're looking for the walkthrough guide on Policies, see: [2.3 Policy](../build/policy.md)*

# 9.4 Policy API

## `class` Policy

A Policy is a special kind of handler that decides whether to forward requests to the Controllers. Trails Policy classes are singletons; that is, each subclassed Policy is instantiated once, and that reference is maintained during the life of the program.

### `constructor (app)`

Initialize the Policy with the provided `app` instance.

| @param | type | description | required? |
|:---|:---|:---|:---|
| `app` | `Trails` | the Trails application instance | yes |

## Methods

None.

## Fields

### `id`

Return the id of the Policy. e.g. for `UserPolicy`, the `id` will be `user`.

### `log`

The application's Logger instance. Convenience reference for `this.app.log`.

### `__`

The i18n translator instance. Convenience reference for `this.app.__`.

### `services`

Convenience reference to `this.app.services`.


## Next: [Service API](service.md)
