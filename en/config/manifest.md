# 3.1. `index.js`

A manifest of the available configuration files. Configuration files added/created should be required in this file. This file should not be used to define any configuration values.

```js
// config/index.js

exports.main = require('./main')
exports.log = require('./log')
exports.routes = require('./routes')
// ... etc
```

## Environment Configuration

Each environment-specific configuration should contain its own manifest file.

```js
// config/env/index.js

exports.staging = require('./staging')
```

```js
// config/env/staging/index.js

exports.main = require('./main')
exports.log = require('./log')
exports.routes = require('./routes')
// ... etc
```
