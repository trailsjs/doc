*Previous: [Model](model.md)*

# 2.5 Resolver

**Resolvers** are to Models as Services are to Controllers. They inform the application how the Model layer interacts with the persistence layer (database). More spefically, they implement the queries that the application is allowed to perform on the Model. The concept of Resolvers is often called a Data Access Object, or [DAO](https://en.wikipedia.org/wiki/Data_access_object).

First, we need to tie off some loose ends from our previous guide on Models. What is `UserResolver`? And how does `user.save()` work?

## `UserResolver`

When creating new Models (via `yo:trails model`), the generator looks at the store config (`config.stores`) to figure out which store the Model will belong to. In our example, we only configured one store. When multiple stores are configured, the generator will ask to which store the new Model will belong.

```es6
// api/resolvers/UserResolver.js

module.exports = class UserResolver extends KnexResolver {

}
```

In most of the Datastore Trailpacks, default Resolvers are provided that implement the following methods:
- `save ()`
- `update (values)`
- `delete ()`
- `get` (criteria)

The `Model` and `Resolver` superclasses (from which `UserModel` and `UserResolver` inherit) bind together and expose these four operations as methods on every `User` subclass by default. The `criteria` will vary in format by store -- your chosen Datastore Trailpack will document how its criteria are used.

## Custom Queries

Of course, we'll often want to do more specific things other than these four common operations. Let's look at an example.

```es6
// api/resolvers/UserResolver.js

  /**
   * Update the user to indicate they've adopted another cat
   * @param criteria  Always the first argument to resolvers; uniquely identifies object
   */
  adoptCat (criteria) {
    return this.store.increment('cats', 1).where(criteria)
  }
```

Now, we expose this functionality to the application by implementing the corresponding method on `UserModel`:

```es6
// api/models/UserModel.js

  /**
   * Adopt a cat
   */
  adoptCat () {
    this.resolver.adoptCat(this.id)
  }
```

## Restricted Operations

Some operations can be disabled by the developer for security or other reasons. For example, read-only Models could inherit from a `ReadOnlyModel` class that does the following:

```es6
// api/models/ReadOnlyModel.js

module.exports = class ReadOnlyModel extends Model {
  update () {
    throw new IllegalAccessError(`${this.getModelName()}.update is not allowed by the server`)
  }
}
```
