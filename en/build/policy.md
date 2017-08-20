#### [Docs](../../) / [Build](./) / Policy

# 2.3 Policy

Policies decide whether a request should invoke the defined Controller at all. They are useful to separate validation logic from request-handling logic. For example, they might perform validation on the request parameters, check for a valid User session, or rate-limit a particular client.

## Example 1: Reject all requests after business hours

As a simple example, let's say your web service should only be available during business hours, 8am - 5pm. We'll write a Policy that validates this condition, and only forwards the request to our Controller only if the request is made during business hours. We'll use [momentjs](https://momentjs.com) to help us with the time validation. Go ahead and `npm install moment`.

Similar to Controllers and Services, we'll create a new Policy using the Trails generator:

### Create Policy

#### `yo trails:policy TimePolicy`

```js
// api/policies/TimePolicy.js

const moment = require('moment')

module.exports = class TimePolicy extends Policy {
  
  static get businessHours () {
    return {
      open: 8,
      close: 17 // 5pm 
    }
  }

  /**
   * Return true if the current time is during business hours
   */
  isDuringBusinessHours (request) {
    const now = moment()
    const { open, close } = TimePolicy.businessHours

    if (now.hours() < open || now.hours() > close) {
      throw new Error('We are currently closed.')
    }

    return true
  }
}
```

### Configure Policy

Let's say we want this Policy to apply to `VersionController.getLatest` that we built in the [previous section](service.md). We configure this Policy as a *precondition* of the `VersionController.getLatest` route:

```js
// config/policies.js

module.exports = [
  {
    VersionController: {
      getLatest: ['TimePolicy.isDuringBusinessHours']
    }
  }
]
```

Now, requests to `/version/{packageName}` will first be validated by `TimePolicy`.

### Try it!

#### At 3pm:

- Request: `GET /version/node`
- Response: 
  ```json
  {
    "node": "v7.5.0"
  }
  ```

#### At 9pm:

- Request: `GET /version/node`
- Response: 
  ```json
  {
    "statusCode": 500,
    "error": "We are currently closed.",
    "message": ""
  }
  ```

### Next: [Model](model.md)
