*Previous: [Policy](policy.md)*

# 2.4 Model

**Models** help organize your data into logical domain objects. Typically, one Model will represent one database table, by defining the schema for how your application data is represented in the database. This concept of Models is often called a Data Transfer Object, or [DTO](https://en.wikipedia.org/wiki/Data_transfer_object).

This guide will walk through how to define our schema using Models. The following guide on Resolvers will build on these ideas and let us store/retrieve data via our Models.

## Choose a Datastore

In Trails, Models are kept in **stores**. A store can be a database, a file on disk, or even an external web service. Trails does not include any native ORM functionality in its core; instead, the developer can install a Trailpack that enables the fuctionality for whichever kind of **store** they plan to use.

### Datastore Trailpacks

#### [trailpack-knex](https://github.com/trailsjs/trailpack-knex)

This Trailpack uses [knex.js](http://knexjs.org/) to connect to most SQL databases, including Postgres, MySQL/MariaDB, SQLite, Oracle, and SQL Server. 

#### [trailpack-sequelize](https://github.com/trailsjs/trailpack-sequelize)

[Sequelize](http://docs.sequelizejs.com/en/v3/) is an ORM (object-relational mapper) that supports Postgres, MySQL/MariaDB, SQLite, and SQL Server.

#### [trailpack-bookshelf](https://github.com/trailsjs/trailpack-bookshelf)

[Bookshelf](http://bookshelfjs.org/) is also an ORM, which uses knex.js under the hood. It supports the same databases as trailpack-knex.

#### [trailpack-waterline](https://github.com/trailsjs/trailpack-waterline)

Waterline is the ORM used by Sails.js, and supports many relational and non-relational (NoSQL) databases.

### Install a Datastore Trailpack

In this guide, we will use **trailpack-knex** to work with a local SQLite database. SQLite is a great way to develop locally with SQL without the complexity of setting up Postgres or MySQL.

Go ahead and install the trailpack and the sqlite3 driver that it will use:

```
npm install trailpack-knex sqlite3 --save
```

Add it to the trailpack list:

```es6
// config/main.js

module.exports = {
  // ...

  packs: [
    // ... ,
    require('trailpack-knex')
  ]

}
```

And configure a store:

```es6
// config/stores.js

module.exports = {

  // as in, trailpack-knex
  knex: {
    devdb: {
      client: 'sqlite3',
      connection: {
        filename: '.tmp/devdb.sqlite'
      }
    }
  }
}
```

## Create a Model

First, we'll use the Trails generator to create a new Model called `User`. Both a Model and a Resolver will be created. The Resolver will be reviewed in section

#### `yo trails:model User`

```es6
// api/models/User.js

const UserResolver = require('../resolvers/UserResolver')

/**
 * @module User
 * @description A User of the application
 */
module.exports = class User extends Model {
	static config () {
    return {
      resolver: UserResolver
    }
	}

	static schema () {

	}
}
```

### Configure the Model Store

For each Model, we specify the store that it belongs to. We created a store called `devdb` above, so we'll use that.

```es6
// api/models/User.js

	static config () {
    return {
      resolver: UserResolver,
      store: 'devdb'
    }
  }
```

### Define the Schema

The Knex Trailpack passes in a [`table`](http://knexjs.org/#Schema-table) argument to the schema method to which we will attach database column definitions according to the [Knex Schema API](http://knexjs.org/#Schema). For our `User` Model, we'll add the following fields:

- `id`: integer, unique, auto increment, primary key
- `email`: string, unique, add index for searching

```es6
// api/models/User.js

  static schema (table) {
    table.increments('id').primary()
    table.string('email').unique().index()
    table.integer('cats')
    table.boolean('hasCats')
  }
```

### Instantiate and Persist a Model

Create a new Model, and persist it into our datastore.

```es6
// api/services/UserService.js

module.exports = class UserService extends Service {

  /**
   * Create a new User given their email, and number of cats they own
   */
  registerUser (email, cats) {
    const user = new this.models.User({
      hasCats: (cats > 0),
      email,
      cats
    })

    // persist "user" to database
    return user.save()
  }
}
```

Simple enough: instantiate a `new this.models.User({ ... })`, and then save it. But, where does `.save()` come from? In the next section, we walk through how Resolvers handle the work of storing and retrieving data to/from the data store.

### Next: [Resolver](resolver.md)
