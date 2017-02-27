#### [Docs](../) / [Build](./) / Policy

# 2.3. Policy

Policies decide whether a request should invoke the defined Controller at all. They are useful to separate validation logic from request-handling logic. For example, they might perform validation on the request parameters, check for a valid User session, or rate-limit a particular client.

## Example 1: Disallow Certain Companies

Building on the Annual Report service from our earlier guides, we'll disallow requests from certain known IP addresses.

As a simple example, let's say your web service should only be available during business hours, 8am - 5pm. We'll write a Policy that validates this condition, and only forwards the request to our Controller only if the request is made during business hours. We'll use [momentjs](https://momentjs.com) to help us with the time validation. Go ahead and `npm install moment`.

Similar to Controllers and Services, we'll create a new Policy using the Trails generator:

### <a href="#create-policy">Create Policy</a>

#### `yo trails:policy ReportPolicy`

```js
// api/policies/ReportPolicy.js

module.exports = class ReportPolicy extends Policy {
  get blacklist () {
    return [
      '0000072859',   // Enron
      '0000723527',   // WorldCom
      '0001413329'    // Philip Morris
    ]
  }

  /**
   * Return true if company requested is evil
   */
  isEvil (request) {
    const { cik } = request.params

    if (this.blacklist.includes(cik)) {
      throw new boom.badRequest('That company is evil!')
    }
  }
}
```

### <a href="#configure-policy">Configure Policy</a>

Let's say we want this Policy to apply to `ReportController.getLatest` that we built in the [previous section](service.md). We configure this Policy as a *precondition* of the `ReportController.getLatest` route:

```js
// config/routes.js

module.exports = [
  {
    method: [ 'GET' ],
    path: '/report/{cik}',
    handler: 'ReportController.getLatest',
    config: {
      pre: [
        'ReportPolicy.isEvil'
      ]
    }
  }
]
```

Now, requests to `/report/{cik}` will first be validated by `ReportPolicy`.

### <a href="#examples">Examples</a>

- Request: `GET /report/0001467858` (General Motors)
- Response: 
  ```json
  {
    "size": "23 MB",
    "filingDate": "2017-02-07",
    "company": {
      "name": "General Motors Co",
      "state": "MI",
      // ...
    },
    // ...
  }
  ```

- Request: `GET /report/0001413329` (Philip Morris)
- Response: 
  ```json
  {
    "statusCode": 500,
    "error": "That company is evil!",
    "message": ""
  }
  ```

### Next: [Model](model.md)
