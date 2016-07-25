# Policies



## What is a Policy

### Introduction
A Policy is <strong>function</strong>, that executed before accessing <strong>Controllers</strong>.
With Policies you can setup <strong>access control</strong>, <strong>authorization</strong> or any <strong>processing</strong> that can be done before accessing controller function.

You can use Policies for whatever you want, there is no convention.

### Examples

#### Authorization Example

For a policy that named isAdmin, we could write a function that allow access if user is admin and return a 403 if user is not an admin.

#### Auth & Processing Example

An other example can be a processing before access the controller, like refreshTokens, that automaticaly refresh authentication tokens for a user, if the user has not disconect from app, have same ip and has an outdated token that is valid, the result can be let accessing controllers or return a 403 if the user has disconected.    

## How to use policies and examples
- [Hapi policies doc](https://github.com/trailsjs/trailpack-hapi/blob/master/docs/concepts/policies/README.md)
- [Express policies doc](https://github.com/trailsjs/trailpack-express/blob/master/docs/concepts/policies/README.md)
