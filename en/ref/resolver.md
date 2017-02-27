#### [Docs](../) / [API Reference](./) / Resolver

##### *If you're looking for the walkthrough guide on Resolvers, see: [2.5 Resolver](../build/resolver.md)*

# 9.7 Resolver API

## `class` Resolver

A Resolver describes the methods necessary for translating data in a persistent store (e.g. database) to/from [Model](model.md) representations. (Sometimes called a [DAO](https://en.wikipedia.org/wiki/Data_access_object)).

## Methods

### `static` `config ()`

Returns the Resolver configuration.

## Fields

### `store`

Type: `Object` or `Function`

The [*handle*](http://docstore.mik.ua/orelly/linux/dbi/ch04_02.htm) used to interact with the underlying datastore. For example, when using the [Knex Trailpack](https://github.com/trailsjs/trailpack-knex), the handle will be the main `knex` object.

```js
  save () {
    return this.store('modules').insert({ name: 'trailsjs' })
  }
```
