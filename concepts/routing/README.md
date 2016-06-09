<img width="250px" align="right" src="../../assets/img/concepts/routing.png">
# Routes

## Introduction
A route is the functionality that implement the possibility to distinguish on URL from an another.<br>
Like all web frameworks, Trails route provide the mechanism that map route to controllers.

## Configuration

### The common way

```JavaScript
// config/routes.js
module.exports = [
  {
    method: [ 'GET' ],
    path: '/example/test',
    handler: 'ExampleController.test'
  }
]
```

#### Required values

##### methods

> Express Methods

- checkout
- copy
- delete
- get
- head
- lock
- merge
- mkactivity
- mkcol
- move
- m-search
- notify
- options
- patch
- post
- purge
- put
- report
- search
- subscribe
- trace
- unlock
- unsubscribe

###### Hapi Methods

You can provide whatever you want in method: can be ['POST', 'GET', 'PUT', 'DELETE', 'OPTIONS', 'PATCH', 'HEAD' ]
These are defined in RFC 2616 (https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

###### Koa Methods

You can provide whatever you want in method: can be ['POST', 'GET', 'PUT', 'DELETE', 'OPTIONS', 'PATCH', 'HEAD' ]
These are defined in RFC 2616 (https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

##### path
The URL that match the case.

##### handler
The controller that handle the request, in most of case: ControllerClass.ControllerFunction

##### Optionals values
> vhost:
Handle the request if the request vhost parameters match.

```JavaScript
// config/routes.js
module.exports = [
  {
    method: [ 'GET' ],
    path: '/example/test',
    handler: 'ExampleController.test',
    vhost: 'www.example.com'
  }
]
```

This part of documentaion is currently under construction.

For more information - Please check the [options available for routing here](https://github.com/trailsjs/trailpack-router/blob/master/lib/schemas/route.js)
