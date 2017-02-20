#### [Docs](../) / Build

##### *If you haven't yet created a Trails Application, [**Start**](../start.md) there first.*

# 2. Build

This guide will walk through adding functionality to a new application, and explain key concepts as we go. We'll define some API methods, implement their business logic, and test them. We refer to this process as API-Driven Development.

## Part 1: Handling Requests

### 2.1. [Controller](controller.md)

The first section of this guide walks through creating a new Controller, implementing handlers, and configuring routes. We'll cover how to build endpoints for both [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) and [GraphQL](http://www.graphql.com/).

### 2.2. [Service](service.md)

Controllers typically invoke Services to perform heavy-lifting tasks that aren't directly related to communicating with the client.

### 2.3. [Policy](policy.md)

Policies decide whether a request should reach a Controller. If you'll be authenticating users, evaluating permissions, or anything like that, you'll want to know how to build Policies.

## Part 2: Dealing with Data

### 2.4. [Model](model.md)

If your application deals with data, you'll want to encapsulate your schema and queries in Models and Resolvers.

### 2.5. [Resolver](resolver.md)

Resolvers are to Models as Services are to Controllers. They take care of the work involved with storing/retrieving data to/from the data store.
