#### [Docs](../) / [Test](./) / Integration Testing

# 4.3. Integration Testing

Integration tests verify that multiple components work together properly. Whereas a Unit Test will focus on an indvidual method, an integration test will verify the result from a complex series of events. In Trails, integration tests will verify the web server will return the correct response given a particular request.

As in previous sections, this section continues the example of the Annual Report web service. Since integration tests verify response for a particular request, integration test suites in Trails are organized by Controller.

- `controllers/ReportController`

## <a href="#testing-controllers">Testing Controllers</a>

Unit test stub files are also created when you generate a new Trails Service. We'll use the `ReportController` example from our walkthrough on building 
[2.2. Service](../build/service).

### <a href="#controller-class">Controller Class</a>

`yo trails:controller ReportController` will generate, along with the specified Controller class, a unit test suite.

```js
// api/controllers/ReportController.js

module.exports = class ReportController extends Controller {
  getLatest (request, reply) {
    // ...
  }
}
```

For reference, the full implementation of this Controller can be found in [2.2 Service](../build/service#implement-reportcontroller).

### <a href="#controller-test-suite">Controller Test Suite</a>

```js
// test/integration/ReportController.test.js

const assert = require('assert')

describe('ReportController', () => {
  let ReportController
  before(() => {
    ReportController = global.app.controllers.ReportController
  })
})
```

### <a href="#create-a-controller-unit-test">Create a Controller Unit Test</a>

In fact, the integration test below will test all of the following application methods:

- [`ReportPolicy.isEvil`](../build/policy#create-policy)
- [`ReportController.getLatest`](../build/service#implement-reportcontroller)
- [`ReportService.getLatest`](../build/service#implement-reportservice)

```js
// test/unit/services/ReportController.test.js

const assert = require('assert')
const supertest = require('supertest')

describe('ReportController', () => {
  let request
  before(() => {
    request = supertest('http://localhost:3000')
  })

  describe('#getLatest', () => {

    it('should return the annual report object for a valid CIK', () => {
      return request.get('/report/0000928465') // Amcon Distributing
        .expect(200)
        .then(res => {
          const report = res.body
          assert.equal(report.name, 'General Motors Co')
        })
    })

    it('should return a 400 error when requesting an evil company', () => {
      return request.get('/report/0000072859') // Enron
        .expect(400)
    })

  })
})
```

