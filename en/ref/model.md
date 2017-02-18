*If you're looking for the walkthrough guide on Models, see: [2.4 Model](../build/model.md)*

# 9.5 Model API

## `class` Model

A Model represents a domain object by defining its schema and backing store. (Sometimes called a [DTO](https://en.wikipedia.org/wiki/Data_transfer_object)).

### `constructor ()`

Initialize the Model, and bind to the specified resolver.

## Methods

### `static` `config ()`

Returns the Model configuration. At minimum, config should contain a reference to the `Resolver`, and specify the backing `store`.

| @return | type | description | required? |
|:---|:---|
| `config.store` | `String` | the name of the backing store (set in `config.stores`) | yes |
| `config.resolver` | `Resolver` | the resolver class to bind to | yes |
| `config.tableName` | `String` | override the default table name | no |

### `static` `schema ([ obj ])`

Returns the Model schema in whichever form is described by the Trailpack that manages this Model's backing store. For example, the [Waterline Trailpack](https://github.com/trailsjs/trailpack-waterline) returns an object from this method; the [Knex Trailpack](https://github.com/trailsjs/trailpack-knex) passes in a `table` object, and attaches the schema to that object.

### `getTableName ()`

Returns the table name derived from the `id`, or from a config override.

## Fields

### `id`

Return the id of the Model. e.g. for a Model called `User`, the `id` will be `user`.

### `resolver`

A reference to this model's `Resolver`.

## Next: [Resolver API](model.md)
