#### [Docs](../../) / [About Trails](./) / What is Trails?  

# 10.1. What is Trails?

Trails is a Modern, Community-driven web application framework for Node.js. Let's break down what we mean by these terms.

### Modern

Trails is designed with modern, best-practices Node.js development patterns in mind. At a technical level, Trails takes greater advantage of the Node.js event system, and utilizes the modern ES6/ES7 dialects of Javascript. It is also based on years of experience maintaining its predecessor, [Sails.js](http://github.com/balderdashy/sails).

### Community-Driven

We strongly believe that the capability of Trails and the priorities of the ecosystem should be driven by the needs and expertise of the Node.js open-source community. The vast majority of the Trails ecosystem collective codebase is authored by outside contributors (i.e. not by the Trails Team), which increases the stability and strength of the ecosystem as a whole.

Many other frameworks require the continued involvement and nurturing of a single person or company for their survival. Trails is not controlled or owned by any individual, however many invested firms do offer [Professional Support](http://trailsjs.io/support) built on their expertise in maintaining the framework. The hyper-modular design of Trails distributes ownership of modules to those indivudals or sponsors who are most self-interested in those modules' continued development and maintenance. Any of over a dozen [Maintainers](https://github.com/orgs/trailsjs/teams/maintainers) can author a new tagged release of Trails, or any of the Trailpacks maintained under the [trailsjs](https://github.com/trailsjs) organization, at any time. See our [Contribution Guidelines](https://github.com/trailsjs/trails/blob/master/.github/CONTRIBUTING.md) for more information.

### Framework

Trails is not a library of libraries. It is not a cargo cult, or a heavy convention-over-configuration monolith. It is, in the purest sense of the word, a *framework*. Trails offers a common configuration schema, and a unified interface for loading plugins. The entire Trails core--including all logic and interfaces--is right around ~1,000 lines of code.

## Why another Node.js framework?

The original authors of Trails, and many of the contributors to this day, were involved for many years in maintaining Sails.js and many of its modules. By 2015, problems were piling up, the original authors had left to work on a new startup, and the community was demanding action. In late 2015, we formed a new team and applied our experience and lessons learned to start Trails.

### Experience

Members of the Trails team were involved with maintaining Sails.js and its ecosystem since 2014. During and after our involvement, we were able to soberly analyze what worked well and what didn't. Moreover, The Trails team authored and maintains many important components of the Sails ecosystem:

#### Sails Hooks / Utilities
- [Sails Permissions](https://github.com/trailsjs/sails-permissions)
- [Sails Auth](https://github.com/trailsjs/sails-auth)
- [Sails Swagger](https://github.com/trailsjs/sails-swagger)
- [Marlinspike](https://github.com/tjwebb/marlinspike)

#### Waterline / Skipper Adapters
- [SQLite3 Adapter](https://github.com/waterlinejs/sqlite3-adapter)
- [PostgreSQL Adapter (Waterline)](https://github.com/waterlinejs/postgresql-adapter)
- [PostgreSQL Adapter (Skipper)](https://github.com/skipperjs/skipper-postgresql)
- [Oracle Adapter](https://github.com/waterlinejs/oracle-adapter)
- [RabbitMQ Adapter](https://github.com/waterlinejs/rabbitmq-adapter)
- [SQL Server Adapter](https://github.com/waterlinejs/sqlserver-adapter)

## Lessons Learned

### Technical

Large, monolithic frameworks such as Sails are a nightmare for maintainers. Dedicated full-time effort from many people is required just to keep the library up-to-date with the evolution of Node.js and dependent modules. Project entropy necessarily grows and compounds over time. 

### Community

The project and ecosystem shouldn't be over-reliant on any single person or entity. Most projects could improve in this area, including Trails, but continued decentralization and democratization of effort and knowledge is a constant goal.

## Why should I use Trails?

- if you're new to Node.js development and want to follow Best Practice
- when you want to build a GraphQL or REST web service
- when you want to render your React / Angular 2 application on the server.
- when you might typically use Hapi to set up a microservices backend, but want a more consistent and organized way of configuring the application.
- are already familiar with Sails, and want to be able to use more modern patterns and techniques
- it will make developing Node.js applications more fun.
- you can deploy it anywhere you normally deploy Node.js applications
- you can get rid of it!

There are plenty of frameworks available already to help you do easy and unimportant things very quickly. Trails helps you build software systems that can scale both in their ability to handle large workloads as well as large development efforts, teams, and project scope. In the infamous cases of [Meteor](https://www.meteor.com/) and [Sails](https://github.com/balderdashy/sails), their disproportionate focus on reducing the development time needed to set up a new app and *see something happen* did not consider the true long-term costs of these design decisions. While Trails happens to be just as easy to set up as other frameworks, we avoid the pitfall of over-optimizing productivity in the early stages of a project. We, and all developers, keenly understand that the vast majority of software costs occur in debugging and maintenance.

We believe that the number of features in a framework is not itself a feature. Our design decisions will always prefer to avoid bloat and complexity, increase usability and developer-friendliness, and respond to user feedback. As a result, applications built with Trails will run faster, require less maintenance effort, and be more fun to build.
