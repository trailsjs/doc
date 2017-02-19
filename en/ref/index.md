##### [Docs](../../) / [API Reference](./)

# 9. API Reference

This section elucidates the inner workings of the Trails framework, and provides a reference for the module APIs.

# Classes

## 9.1. [Trails](trails.md)

This is the primary API that your application resources, such as Controllers and Services, will use to communicate with other areas of your application.

## 9.2. [Trailpack](trailpack.md)

This interface is implemented by all Trailpacks, and is relied upon by the Trails Application to harmoniously load and manage them.

## 9.3. [Controller](controller.md)

Trails Controllers contain request handler methods. Controller methods receive requests from the client, and send responses to them.

## 9.4. [Policy](policy.md)

A Policy is a special kind of handler that decides whether to forward requests to the Controllers.

## 9.5. [Service](service.md)

Trails Services contain most of the application's business logic. Service methods are invoked by Controller handlers to process requests.

## 9.6. [Model](model.md)

A Model represents a domain object by defining its schema and backing store.

## 9.7. [Resolver](resolver.md)

A Resolver describes the methods necessary for translating data in a persistent store (e.g. database) to/from [Model](model.md) representations.
