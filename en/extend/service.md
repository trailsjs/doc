#### [Docs](../../) / [Extend](./) / Service

# 7.1 Services

In Trails, [Services](../../ref/service) can be peeled off of the main application and put into separate modules.
This allows you to easily organize your project into modular microservices, and include Services into other projects that may require shared functionality.

## Include a Service in your Application

Typically, your `api/services` folder contains a number of `Service` classes.
You can include a Service from another library or application simply by using Node.js `require()`.
First, install the module:

```sh
npm install @myorg/api/services/TranslationService
```
Then, include it into the Services manifest:

```js
// api/services/index.js

exports.TranslationService = require('@myorg/api/services/TranslationService')
```

## Peel off an Existing Service into a Separate Module

Your Trails application will have many Services that serve as helpers to the Controller handlers, and to execute business logic.
Often, you'll want to share one or more Service with another application. Let's walk through this process with an example.

### Existing Application Service

Suppose we've written a Service that identifies and translates text into English, and we want to re-use this Service across many applications.

```js
// api/services/TranslationService.js
const LanguageTranslatorV2 = require('watson-developer-cloud/language-translator/v2')

/**
 * Services to assist in translating text into English using IBM Watson
 */
module.exports = class TranslationService extends Service {

  /**
   * Determine the source language of a given input string, then translate
   * into English.
   */
  translate (text) {
    return new Promise((resolve, reject) => {
      this.translator.identify({ text }, (err, source) => {
        this.translator.translate({ text, source, target: 'en' }), (err, translation) => {
          if (err) return reject(err)
          resolve(translation)
       })
     })
    })
  }

  constructor (app) {
    this.translator = new LanguageTranslatorV2({
      // watson credentials ...
    })
  }
}
```

### Create a new Node Module

Here, we'll create a vanilla node module as we normally would when building a shared node module.

1. Create a new Project Directory
  ```sh
  mkdir -p translator-service/api/services
  cd translator-service
  ```

2. Run `npm init` to generate a new module

```sh
$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (translator-service)
version: (1.0.0)
description: Translator Service
entry point: (index.js) api/services/TranslatorService.js
test command: eslint . && mocha
git repository:
keywords:
author: Trails Team <hello@trailsjs.io>
license: (ISC) MIT
About to write to /home/trailsjs/translator-service/package.json:

{
  "name": "translator-service",
  "version": "1.0.0",
  "description": "Translator Service",
  "main": "api/services/TranslatorService.js",
  "scripts": {
    "test": "eslint . && mocha"
  },
  "author": "",
  "license": "MIT"
}


Is this ok? (yes) yes
```

3. `require()` the module as demonstrated above!

### Next: [Trailpack](./trailpack.md)