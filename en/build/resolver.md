#### [Docs](../) / [Build](./) / Resolver

# 2.5. Resolver

**Resolvers** are to Models as Services are to Controllers. They inform the application how the Model layer interacts with the persistence layer (database). More spefically, they implement the queries that the application is allowed to perform on the Model. The concept of Resolvers is often called a Data Access Object, or [DAO](https://en.wikipedia.org/wiki/Data_access_object).

First, we need to tie off some loose ends from our previous guide on Models. What is `CompanyResolver`? And how does `user.save()` work?

## <a href="#companyresolver">`CompanyResolver`</a>

When creating new Models (via `yo:trails model`), the generator looks at the store config (`config.stores`) to figure out which store the Model will belong to. In our example, we only configured one store. When multiple stores are configured, the generator will ask to which store the new Model will belong.

```js
// api/resolvers/CompanyResolver.js

module.exports = class CompanyResolver extends KnexResolver {

}
```

In most of the Datastore Trailpacks, default Resolvers are provided that implement the following methods:
- `save (values)`
- `update (criteria, values)`
- `delete (criteria)`
- `get` (criteria)

The `Model` and `Resolver` superclasses (from which `Company` and `CompanyResolver` inherit) bind together and expose these four operations as methods on every `Company` subclass by default. The `criteria` will vary in format by store -- your chosen Datastore Trailpack will document how its criteria are used.

### <a href="#override-existing-query">Override Existing Query</a>

Let's override the existing `.save()` method so that we can store our parsed RSS feed from [2.2 Service](./service#implement-reportservice) directly.

For reference, here's a snippet of the parsed RSS feed for General Motors:

```json
{
  "companyInfo": {
    "addresses": [
      {
        "type": "business",
        "city": "DETROIT",
        "phone": "313.556.5000",
        "state": "MI",
        "street1": "300 RENAISSANCE CENTER",
        "zip": "48265-3000"
      }
    ],
    "assignedSicDesc": "MOTOR VEHICLES & PASSENGER CAR BODIES",
    "cik": "0001467858",
    "conformedName": "General Motors Co",
    "fiscalYearEnd": "1231"
  }
}
```

```js
// api/resolvers/CompanyResolver.js

  /**
   * @override
   * Extract and format the company data from the RSS feed object, and insert
   * a new Company record.
   */
  save ({ companyInfo }) {
    const {
      conformedName: name,
      assignedSicDesc: description,
      cik,
      isEvil    // set by CompanyService.registerCompany
    } = companyInfo
    const address = companyInfo.addresses.find(addr => addr.type = 'business')
    const { city, phone, state, street1: street, zip } = address

    return this.store.insert({
      name, description, cik, isEvil, city, phone, state, street, zip
    })
  }
```

Now, calls to `Company.save()` will invoke this new Resolver method.

### Restrict Queries

Some operations can be disabled by the developer for security or other reasons. For example, read-only Models could inherit from a `ReadOnlyModel` class that does the following:

```js
// api/models/ReadOnlyModel.js

module.exports = class ReadOnlyModel extends Model {
  update () {
    throw new IllegalAccessError(`${this.getModelName()}.update is not allowed by the server`)
  }
}
```

### <a href="#custom-queries">Custom Queries</a>

Of course, we'll often want to do more specific things other than the four common CRUD (create, read, update, delete) operations. In the previous section, we disabled the `.update()` method, but occasionally a company will turn evil. 

```js
// api/resolvers/CompanyResolver.js

  /**
   * Relocate the company to a new address. If the company is moving to an
   * off-shore tax haven, it becomes evil.
   */
  relocate (criteria, newAddress) {
    const patch = { ...newAddress }

    if (/Grand Cayman/i.test(newAddress.street1)) {
      patch.isEvil = true
    }

    return this.store('company').where(criteria).update(patch)
  }
```

Now, we expose this functionality to the application by implementing the corresponding method on `CompanyModel`:

```js
// api/models/Company.js

module.exports = class Company extends Model {
  // ... 

  relocate (newAddress) {
    return this.resolver.relocate(this, newAddress)
  }
}
```

