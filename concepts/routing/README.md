# Routes



## What is a Route

### Introduction
A route is the functionality that implement the possibility to distinguish on URL from an another.
Like all web frameworks, Trails route provide the mechanism that map route to controllers.

## How to use it in Trails

#### 1 - Configuration

##### The common way

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

###### Required values

> method: can be ['POST', 'GET', 'PUT', 'DELETE']
These are defined in RFC 2616 (https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

> path: The URL that match the case.

> handler: The controller that handle the request, in most of case: ControllerClass.ControllerFunction

###### Optionals values
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
