#### [Docs](../) / [Test](./) / Overview

# 4.1. Testing Overview

## <a href="#new-application">New Application</a>

When you create a new application using `yo trails`, a test suite will be generated with the following structure:

- test/index.js
- test/unit/
- test/integration/

Individual test suites are automatically created when the trails generator is used to create a new Model, Service, Controller, Policy, or Resolver. All test suite files will end in `.test.js`. [Fixtures](https://github.com/junit-team/junit4/wiki/test-fixtures) and other helper files that do not contain test suites should be suffixed normally with `.js`.

## Mocha.js

By default, Trails uses [mocha](https://mochajs.org/) for its test suites. It may help to become familiar with Mocha a bit before proceeding, but it is not required.

#### <a href="test-indexjs">`test/index.js`</a>

This tests the basic functioning of the application, and sets a global variable called `app` that the test suites can use to test various aspects of the application.

```js
// test/app.js

const Trails = require('trails')

before(() => {
  global.app = new Trails(require('../'))
  return global.app.start().catch(global.app.stop)
})
after(() => {
  return global.app.stop()
})
```

#### <a href="test-unit">`test/unit/`</a>

The unit test suites for Services, Policies, and Models.

#### <a href="test-integration">`test/integration/`</a>

The integration test suites for Controllers and Resolvers. Occasionally, some service methods will require integration tests as well.

### Next: [Unit Testing](unit.md)
