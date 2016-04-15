# Policies



## What is a Policy

### Introduction
A Policy is <strong>function</strong>, that executed before accessing <strong>Controllers</strong>.
With Policies you can setup <strong>access controll</strong>, <strong>autorization</strong> or any <strong>processing</strong> that can be done before accessing controller function.

You can use Policies for whatever you want, there is no convention.

### Examples

#### Authorization Example

For a policy that named isAdmin, we could write a function that allow access if user is admin and return a 403 if user is not an admin.

#### Auth & Processing Example

An other example can be a processing before access the controller, like refreshTokens, that automaticaly refresh authentication tokens for a user, if the user has not disconect from app, have same ip and has an outdated token that is valid, the result can be let accessing controllers or return a 403 if the user has disconected.    

## How to use it in Trails

### Fastest Way : Use Yeoman

```Bash
  yo trails:policy ExamplePolicy
```

With Yeoman you don't need to create the file and include it in index.js

### First Step : Write your policy

The convention is that policies has to be written in api/policies.

#### 1 - Using Hapi

```JavaScript
  'use strict'
  const Policy = require('trails-policy')

  /**
   * @module ExamplePolicy
   * @description This is the example policy
   */
  module.exports = class ExamplePolicy extends Policy {
    test(request, reply) {
      const secret = request.params.secret
      // Your own logic
      /**
      * The following code is an example:
      * It stop when the secret parametter is passed and continue to the Controller when there is no secret parametter.
      */
      if (secret != '') {
        // Stop and return with a custom message
        return reply('Your secret is' + secret)
      }
      // Continue to Example Controller
      reply()
    }
  }
```

#### 2 - Using Express

```JavaScript
'use strict'
 const Policy = require('trails-policy')

 /**
  * @module ExamplePolicy
  * @description This is the example policy
  */
 module.exports = class ExamplePolicy extends Policy {
   test(req, res, next) {
     const secret = req.body.secret
     // Your own logic
     /**
     * The following code is an example:
     * It stop when the secret parametter is passed and continue to the Controller when there is no secret parametter.
     */
     if (secret != '') {
       // Stop and return with a custom message
       return res.send('Your secret is' + secret)
     }
     // Continue to Example Controller
     next()
   }
 }
```
### Second Step : Load your policy

```JavaScript
/**
 * FILE: api/policies/index.js
 */

 'use strict'

 module.exports = {}
 exports.Example = require('./Example')

```

### Third Step : Declare your policy in config the file

```JavaScript
/**
 * FILE: config/policies.js
 * The following Policies are example of what you can do with Trails Policies
 */

 module.exports = {

   // Apply on every controllers
   '*': ['ExamplePolicy.test'],

   ExampleController: {
     // Unique policy for one controller function
     test: [ 'ExamplePolicy.test' ],
     // Before accessing multiple function in ExampleController, processing multiple policies example:
     multiple: ['ExamplePolicy.test', 'ExamplePolicy.testfoo', 'ExamplePolicy.testbar'],
   },
   /**
    * Controller Policies that applies on each function under a specific controller
    */

   DefaultController: ['AccessPolicy.logger'],
   /**
    * Boolean & wilcard policies
    * true -> Unrestricted access
    * False -> Restrict Every Time
    */

    CatsController: {
      // Always access to controller function
      miaou: true,
      // Restrict access to controller function
      purr: false,
      // Apply every policies processing before accessing function
      test: '*'
    }
 }
```
