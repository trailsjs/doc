- *If you haven't yet created a Trails Application, [**Start**](../start.md) there first.*
- *This is a walkthrough guide which illustrates key concepts. If you're looking for the API docs, see the [**API Reference**](../ref/README.md) instead.*

# 2. Build

This guide will walk through adding functionality to a new application, and explain key concepts as we go. We'll define some API methods, implement their business logic, and test them. We refer to this process as API-Driven Development.

## Part 1: Handling Requests

### [Controller](controller.md)

The first section of this guide walks through creating a new Controller, implementing handlers, and configuring routes.

### [Service](service.md)

Controllers typically invoke Services to perform heavy-lifting tasks that aren't directly related to communicating with the client.

### [Policy](policy.md)

Policies decide whether a request should reach a Controller. If you'll be authenticating users, evaluating permissions, or anything like that, you'll want to know how to build Policies.

## Part 2: Dealing with Data

### [Model](model.md)

If your application deals with data, you'll want to encapsulate your schema in Models.

### [Resolver](resolver.md)

Resolvers are to Models as Services are to Controllers. They inform the application how the Model layer interacts with the persistence layer (database).
