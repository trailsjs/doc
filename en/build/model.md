#### [Docs](../) / [Build](./) / Model

# 2.4. Model

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

```js
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

```js
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

First, we'll use the Trails generator to create a new Model called `Company`. Both a Model and a Resolver will be created. The Resolver guide is in the next section: [2.5 Resolver](./resolver)

#### `yo trails:model Company`

```js
// api/models/Company.js

const CompanyResolver = require('../resolvers/CompanyResolver')

/**
 * @module Company
 * @description A Company
 */
module.exports = class Company extends Model {
  static config () {
    return {
      resolver: CompanyResolver
    }
  }

  static schema () {

  }
}
```

### Configure the Model Store

For each Model, we specify the store that it belongs to. We created a store called `devdb` above, so we'll use that.

```js
// api/models/Company.js

  static config () {
    return {
      resolver: CompanyResolver,
      store: 'devdb'    // <----------
    }
  }
```

### <a href="#define-the-schema">Define the Schema</a>

The Knex Trailpack passes in a [`table`](http://knexjs.org/#Schema-table) argument to the schema method to which we will attach database column definitions according to the [Knex Schema API](http://knexjs.org/#Schema). For our `Company` Model, we'll add the following fields:

- `id`: integer, unique, auto increment, primary key
- `email`: string, unique, add index for searching

```js
// api/models/Company.js

module.exports = class Company extends Model {
  // ...

  static schema (table) {
    table.increments('id').primary()
    table.string('cik').unique().index()
    table.string('name')
    table.string('description')
    table.string('city')
    table.string('state')
    table.string('street')
    table.string('zip')
    table.boolean('isEvil')
  }

  /**
   * The CIK must always be exactly ten digits; left-pad with zeroes
   * if less than ten digits.
   * @see https://en.wikipedia.org/wiki/Central_Index_Key
   */
  static formatCIK (cik) {
    return leftPad(cik, (10 - cik.length), '0')
  }

  /**
   * @override
   */
  get cik () {
    return Company.formatCIK(this.data.cik)
  }
```

### Instantiate and Persist a Model

Create a new Model, and persist it into our datastore.

```js
// api/services/CompanyService.js

module.exports = class CompanyService extends Service {

  /**
   * Create a new Company, and store a flag that indicates whether it is evil
   * @param rss Object  an object that represents the RSS feed from EDGAR
   */
  registerCompany (rss) {
    const blacklist = this.app.policies.ReportPolicy.blacklist

    const company = new this.models.Company({
      ...values,
      isEvil: blacklist.includes(values.cik)
    })

    // persist "company" to database
    return company.save()
  }
}
```

We instantiate a `new this.models.Company({ ... })`, and then save it. In the process, we also check [`ReportPolicy.blacklist`](./policy#create-policy) to see if we should set the `isEvil` flag. But, where does `.save()` come from, and how does it work? In the next section, we walk through how Resolvers handle the work of storing and retrieving data to/from the data store.

### Next: [Resolver](resolver.md)
