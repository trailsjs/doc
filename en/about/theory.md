#### [Docs](../../) / [About Trails](./) / Theory

# 10.2 Design Theory

## Overview

**The Trails framework has exactly one feature: the ability to harmoniously manage Trailpacks.** The capability available to the developer is thus an emergent property of the Trailpacks the user has installed. This article describes the reasoning behind this design and how it facilitates both modularity and extensibility.

Trails is **not** a software library of libraries, e.g. [Spring](https://en.wikipedia.org/wiki/Spring_Framework), [Rails](https://en.wikipedia.org/wiki/Ruby_on_Rails), Sails.js, which necessarily accrues bloat and entropy over time as features are added and modified. In these systems, modularity is sacrificed for extensibility, resulting in highly centralized and monolithic systems. Trails behaves much more like an operating system kernel, which manages resources and defines interfaces through which the resources can communicate with one another. A particular combination of Trailpacks in a Trails application can be thought of similarly to the ecosystem of Linux distributions. The underlying kernel is "Linux", but the packages included and the functionality exposed to the user is customized by the distribution.

## Trailpack Lifecycle

Trails defines the an interface called the Trailpack Lifecycle which inform how Trailpacks are loaded and managed.

### Lifecycle Stages

Trailpacks are loaded in three stages: Validate, Configure, and Initialize. All Trailpacks must complete each stage before the next stage begins.

### Lifecycle Configuration

Each Trailpack declares a set of events as preconditions and postconditions for each Lifecycle Stage. This allows Trails to determine the optimal load order for a set of Trailpacks. 

### Lifecycle API

Each Trailpack implements a method corresponding to each lifecycle stage.
- [validate](https://github.com/trailsjs/trailpack#validate)
- [configure](https://github.com/trailsjs/trailpack#configure-1)
- [initialize](https://github.com/trailsjs/trailpack#initialize)

A Trailpack can do anything it wishes. [trailpack-hapi](https://github.com/trailsjs/trailpack-hapi), for example, configures and starts up a [Hapi](https://hapijs.com/) server; [trailpack-graphql](https://github.com/langateam/trailpack-graphql) compiles a GraphQL schema and accepts GraphQL queries.

## Theory

The design of the Trailpack loading system is based on both practical and academic experience. This section explores some of the theoretical backing of the Trailpack system design. 

### Node EventEmitter

Trails applications are, in object-oriented programming terminology, instances of the [`TrailsApp`](https://github.com/trailsjs/trails/blob/master/index.js#L11) type. `TrailsApp` is a subtype of [`EventEmitter`](https://nodejs.org/api/events.html#events_class_eventemitter). Trails applications thus inhabit a self-contained event-space via which the Trailpacks can communicate with one another, and with the `TrailsApp` instance itself.

### The Tuple Space

The process of loading and managing the execution of Trailpacks is governed by a modified implementation of a [tuple-space](https://en.wikipedia.org/wiki/Tuple_space) data structure. A tuple-space consists of a single shared object space, and a collection of producers/consumers which read and deposit "tuples" to/from the shared space. In our case, the shared space is provided naturally by the `EventEmitter` type from which `TrailsApp` inherits, and the Trailpacks are the producers/consumers of events. By specifying its preconditions/postconditions in the form of event names, each Trailpack provides to the system the information it needs to correctly and optimally load the Trailpacks into the system.

### Inversion of Control

Trails is designed around the paradigm of [Inversion of Control](https://en.wikipedia.org/wiki/Inversion_of_control), which utilizes the Tuple Space and Node Event Loop together as a method of control flow, and relies on the Trailpacks themselves to implement the essential program logic.
