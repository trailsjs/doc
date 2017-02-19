#### [Docs](../../) / [Configuration](./) / stores.js

# 3.5. `stores.js`

Define data stores. Each [Model](../build/model.md) maps itself to a "store". A single Trails application can configure many data stores of similar or different types.

```js
// config/stores.js

module.exports = {

  /**
   * A store named "localdisk" that uses the Waterline ORM
   */
  roledb: {
    adapter: require('waterline-sqlite3'),
    migrate: 'alter'
  },

  /**
   * A store named "localpostgres" that uses Knex.js
   */
  userdb: {
    client: 'pg',
    connection: {
      host: 'localhost',
      user: 'trails',
      password: 'parkranger',
      database: 'trails'
    }
  }

}
```

The configuration makes use of functionality provided by [trailpack-waterline](https://github.com/trailsjs/trailpack-waterline) and [trailpack-knex](https://github.com/trailsjs/trailpack-knex).

## `roledb`

This store defines an `adapter` property, which the Waterline Trailpack will use to establish a connection to a local SQLite database. [Full Waterline Trailpack Docs](https://github.com/trailsjs/trailpack-waterline).

## `userdb`

This store defines `client` and `connection` properties, which the Knex Trailpack will use to establish a connection to a Postgres database. [Full Knex Trailpack Docs](https://github.com/trailsjs/trailpack-knex).

# Usage

## Models

Each Model defines its backing store. To illustrate how multiple stores can be used, we'll store Users in the Postgres database, and we'll store associated Roles in the SQLite3 database.

```js
// api/models/User.js

module.exports = class User extends Model {
  static config () {
    return {
      store: 'userdb'
    }
  }

  /**
   * The Knex Trailpack sends us a Knex "table" object to define the schema
   */
  static schema (table) {
    table.increments('id').primary()
    table.string('username')
  }
}
```

```js
// api/models/Role.js

module.exports = class Role extends Model {
  static config () {
    return {
      /**
       * Override table name. By default, database tables/collections are named
       * after the Model name. (i.e., "role" by default)
       */
      tableName: 'userroles',
      store: 'roledb'
    }
  }

  /**
   * The Waterline Trailpack requires that a Waterline schema definition object
   * is returned
   */
  static schema () {
    return {
      id: {
        primaryKey: true
      },
      name: {
        type: 'string',
        notNull: true
      },
      user: {
        type: 'integer',
        index: true
      }
    }
  }
}
```

## Services

```js
// api/services/PermissionService.js

module.exports = class PermissionService extends Service {
  /**
   * Return true if the given User has the required role; false otherwise.
   */
  isPermitted (user, requiredRole) {
    return this.app.orm.Role
      .find({
        user: user.id,
        name: requiredRole
      })
      .then(([ role ]) => return !!role)
  }
}
```
