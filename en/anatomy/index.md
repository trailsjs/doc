# Anatomy
###### Trails Anatomy

In Trails files are not loaded automatically, each files that you want to be part of the project need to be declare under the 
`index.js` in the same folder.
 
## Example : 
If you have the following config folder : 

:file_folder: **config** <br>
&nbsp;&nbsp;&nbsp;&nbsp;:file_folder: **env** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;development.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;index.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;production.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;staging.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;testing.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;database.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;main.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;routes.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;index.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;log.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;policies.js <br>
&nbsp;&nbsp;&nbsp;&nbsp;web.js <br>

The `config/index.js` must look like : 

```
exports.database = require('./database')
exports.env = require('./env')
exports.log = require('./log')
exports.main = require('./main')
exports.policies = require('./policies')
exports.routes = require('./routes')
exports.web = require('./web')
```
If you miss one of those declarations, the file will not be loaded into Trails instance and if will no be accessible to the project.

The `env` folder works the same way and `require('env')` will load the `env/index.js` file witch include all env files.