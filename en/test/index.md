#### [Docs](../) / Test

# 4. Test

Verifiability is one of the most important concerns in any software system, and Trails makes it easy to test your code.

## 4.1. [Overview](overview.md)

An overview of how test suites are organized. Read this first if you are new to Trails.

## 4.2. [Unit Testing](unit.md)

Unit tests verify the functionality of individual methods and components that typically don't require interaction with external resources (e.g., databases). If exernal resources are needed, they are usually [mocked](https://en.wikipedia.org/wiki/Mock_object#Use_in_test-driven_development).The following Trails classes should be unit tested:

- Policies
- Services
- Models
- Resolvers

## 4.3. [Integration Testing](integration.md)

Integration tests verify that multiple components work together properly. Whereas a Unit Test will focus on an indvidual method, an integration test will verify the result from a complex series of events. In Trails, integration tests will verify the web server will return the correct response given a particular request.
