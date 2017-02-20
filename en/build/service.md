#### [Docs](../) / [Build](./) / Service

# 2.2 Service

Services are invoked by Controller handlers to perform more complex business logic not directly related to client communication.

## Query an External Service

Let's build a simple web service that returns the latest version of some popular softtware packages as JSON. As in the previous section, we'll create a Controller that handles the client request/response. We'll additionally create a Service that the Controller will invoke to query the version of a package.

#### `yo trails:controller VersionController`
#### `yo trails:service VersionService`

### Implement the Controller

```js
// api/controller/VersionController.js

module.exports = class VersionController extends Controller {

  /**
   * Return the latest version of a given software package
   * @param request.params.packageName
   */
  getLatest (request, reply) {
    const { packageName }  = request.params

    this.services.VersionService.getLatest(packageName)
      .then(version => {
        return reply({
          [packageName]: `v${version}`
        })
      })
      .catch(err => reply(err))
  }
}
```

### Implement the Service

`VersionService` will request version information from [semver.io](http://semver.io), an external web service that hosts version information on nodejs, npm, yarn, and other tools. Separating out this business logic from the request-handling duties of the Controller is a good practice for separating concerns and organizing code.

```js
// api/services/VersionService.js

const request = require('request-promise')

module.exports = class VersionService extends Service {

  static get supportedPackages () {
    return [
      'node',
      'npm',
      'yarn',
      'nginx'
    ]
  }

  /**
   * Query the semver.io service for the latest version of a specified package name
   * @param packageName
   */
  getLatest (packageName) {
    if (!VersionService.supportedPackages.find(packageName)) {
      throw new Error(`${packageName} not supported`)
    }

    return request(`semver.io/${packageName}`)
  }
}
```

### Configure the Route

```js
// config/routes.js
module.exports = [
  {
    method: [ 'GET' ],
    path: '/version/{packageName}',
    handler: 'VersionController.getLatest'
  }
]
```

### Try it!

- Request: `GET /version/node`
- Response: 
  ```json
  {
    "node": "v7.5.0"
  }
  ```

- Request: `GET /version/npm`
- Response: 
  ```json
  {
    "node": "v4.2.0"
  }
  ```

- Request: `GET /version/fancypackage`
- Response: 
  ```json
  {
    "statusCode": 500,
    "error": "fancypackage not supported",
    "message": ""
  }
  ```

### Next: [Policy](policy.md)
