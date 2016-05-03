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
> method: can be ['POST', 'GET', 'PUT', 'DELETE']
These are defined in RFC 2616 (https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

> path: The URL that match the case.

> handler: The controller that handle the request, in most of case: ControllerClass.ControllerFunction

##### Execute Something before accessing Controller
```JavaScript
{
  method: [ 'GET' ],
  path: '/example/test',
  handler: 'ExampleController.test',
  config: {
    pre: [ 'ExamplePreClass.test' ]
  }
}
```
> pre: Execute the function before the controller
