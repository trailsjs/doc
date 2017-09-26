#### [Docs](../) / [Test](./) / Unit Testing

# 4.2. Unit Testing

Unit tests verify the functionality of individual methods and components that don't require interaction with external resources (e.g., databases. In this section, similar to previous sections, we'll illustrate testing by building on the use case of a web service that provides the annual reports for large companies to subscribers.

- `models/Company`
- `resolvers/CompanyResolver`
- `services/ReportService`
- `policies/ReportPolicy`

## <a href="#testing-services">Testing Services</a>

Unit test stub files are also created when you generate a new Trails Service. We'll use the `ReportService` example from our walkthrough on building Services. [2.2. Service](../build/service).

### <a href="#service-class">Service Class</a>

`yo trails:service ReportService` will generate, along with the main service class, a unit test suite.

```js
// api/services/ReportService.js

module.exports = class ReportService extends Service {
  /**
   * Parse the RSS feed and convert to JSON
   */
  parseFeed (feed) {
    // ...
  }
}
```

### <a href="#service-test-suite">Service Test Suite</a>

```js
// test/unit/services/ReportService.test.js

const assert = require('assert')

describe('ReportService', () => {
  let ReportService
  before(() => {
    ReportService = global.app.services.ReportService
  })
})
```

### <a href="#create-a-service-unit-test">Create a Service Unit Test</a>

For reference, here is an example of the feed we want to parse: [RSS Feed](https://gist.github.com/tjwebb/619ffedec259f4d2afe6c9038b93213a#file-sec-rss-feed-xml). We'll create a *fixture* directory called `testfeeds/` and store this, and other test feed data, in that directory.


```js
// test/unit/services/ReportService.test.js

const assert = require('assert')
const testfeeds = require('./testfeeds')

describe('ReportService', () => {
  // ...
  describe('#parseFeed', () => {
    it('should parse a typical SEC feed into JSON', () => {
      const feed = ReportService.parseFeed(testfeeds.generalMotors)
      assert(typeof feed == 'object')
    })
    it('should parse the RSS feed <company-info> section', () => {
      const feed = ReportService.parseFeed(testfeeds.generalMotors)
      assert.equal(feed.conformedName, 'General Motors Co')
      assert.equal(feed.companyInfo.cik, '0001467858')
    })
  })
})
```

## <a href="#testing-policies">Testing Policies</a>

In all of the official Trails guides, we use the [Hapi Trailpack](https://github.com/trailsjs/trailpack-hapi) to build web services. We thus will employ, in this section, a technique for unit testing Policies that is specific to Hapi. For Express, [supertest](https://github.com/visionmedia/supertest) is an excellent tool for unit testing.

### <a href="#policy-class">Policy Class</a>

Remember in [2.3 Policy](../build/policy) we implemented a method called `isEvil`.

```js
// api/policies/ReportPolicy.js

module.exports = class ReportPolicy extends Policy {
  isEvil (request) {
    // ...
  }
}
```

### <a href="#policy-test-suite">Policy Test Suite</a>

```js
// test/unit/policies/ReportPolicy.test.js

const assert = require('assert')

describe('ReportPolicy', () => {
  let ReportPolicy, server
  before(() => {
    ReportPolicy = global.app.services.ReportPolicy
    server = global.app.packs.hapi.server
  })
})
```

### <a href="#create-a-policy-unit-test">Create a Policy Unit Test</a>

```js
// test/unit/policies/ReportPolicy.test.js

const assert = require('assert')

describe('ReportPolicy', () => {
  // ...
  it('should reject Annual Report requests for Philip Morris', () => {
    return server.inject({ method: 'GET', url: '/report/0001413329' })
      .then(res => {
        assert.equal(res.statusCode, 400)
      })
  })
  it('should accept requests for non-evil companies', () => {
    return server.inject({ method: 'GET', url: '/report/0001467858' })
      .then(res => {
        assert.equal(res.statusCode, 200)
      })
  })
})
```

## <a href="#testing-models">Testing Models</a>

`yo trails:model Company` will generate, along with the specified Model and Resolver classes, unit test suites:

- `test/unit/models/Company.test.js`
- `test/unit/resolvers/CompanyResolver.test.js`

### <a href="#model-class">Model Class</a>

The `Company` Model is summarized below in pseudo-code.See [2.4 Model](../build/model#define-the-schema) for the full Model class definition.

```js
// api/models/Company.js

module.exports = class Company extends Model {
  /**
   * Fields:
   *  - name (string)
   *  - description (string)
   *  - cik (string; central index key)
   *  - city (string)
   *  - state (string)
   *  - street (string)
   *  - zip (string)
   */

  formatCIK (cik) {
    // ...
  }
}
```

### <a href="#model-test-suite">Model Test Suite</a>

```js
// test/unit/models/Company.test.js

const assert = require('assert')

describe('Company', () => {
  let Company
  before(() => {
    assert(global.app.models.Company)

    Company = global.app.models.Company
  })
})
```

### <a href="#create-a-model-unit-test">Create a Model Unit Test</a>

Let's test the `formatCIK` method to ensure it works as intended.

```js
// test/unit/models/Company.test.js

const assert = require('assert')

describe('Company', () => {
  // ...
  describe('#formatCIK', () => {

    it('should always return a ten-digit string', () => {
      assert.equal(Company.formatCIK('928465').length, 10)
      assert.equal(Company.formatCIK('0928465').length, 10)
      assert.equal(Company.formatCIK('0000928465').length, 10)
    })

    it('should format cik when setting values via the constructor', () => {
      const company = new Company({ cik: '928465' })
      assert.equal(company.cik, '0000928465')
    })

  })
})
```

### Run the Test!

Just execute `npm test` in your console, and you'll see an output like this:

```
  2 passing (5ms)
```

## <a href="#testing-resolvers">Testing Resolvers</a>

In a Trails application, Resolver methods are only ever invoked by the Models they are coupled with. As such, Resolvers tests occur within the respective Model test suite.

### <a href="#resolver-class">Resolver Class</a>

`CompanyResolver` is summarized below in pseudo-code. See [2.5 Resolver](../build/resolver#override-existing-query) for full class definition.

```js
// api/resolvers/CompanyResolver.js

module.exports = class CompanyResolver extends KnexResolver {

  /**
   * @override
   * Extract and format the company data from the RSS feed object, and insert
   * a new Company record.
   */
  save () {
    // ...
  }

  /**
   * Relocate the company to a new address. If the company is moving to an
   * off-shore tax haven, it becomes evil.
   */
  relocate (newAddress) {
    // ...
  }
}
```

### <a href="#create-a-resolver-unit-test">Create a Resolver Unit Test</a>

```js
// test/unit/models/Company.test.js

const assert = require('assert')

describe('Company', () => {
  // ...

  describe('#relocate', () => {

    it('should update the Company address', () => {
      const company = new Company({ cik: '0000000000' })
      return company.relocate({ city: 'Norfolk', state: 'VA' })
        .then(company => {
          assert.equals(company.address.city, 'Norfolk')
        })
    })

  })

  describe('#save', () => {
    it('should accept an RSS feed object and save it', () => {

      const rssObject = ...
      const company = new Company({ cik: '0000001234' })
      return company.save()
        .then(company => {
          return Company.get({ cik: '0000001234' })
        })
        .then(newCompany => {
          assert(newCompany)
          
        })
    })
  })

})
```

### Next: [Integration Testing](integration.md)
