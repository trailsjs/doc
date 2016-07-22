# Anatomy
###### Trails Anatomy
This folder describe the anatomy of a Trails application.

```
- api
  - controllers 
      DefaultController.js 
      ViewController.js    
      index.js
  - models 
      ExampleModel.js
      index.js
  - policies  
      ExamplePolicy.js
      index.js
  - services
      DefaultService.js 
      index.js
    index.js
- config
  - locales
      en.json
  - env
      development.js 
      index.js       
      production.js  
      staging.js     
      testing.js
  database.js 
  i18n.js          
  main.js     
  routes.js   
  views.js
  index.js    
  log.js      
  policies.js 
  session.js  
  web.js
- node_modules
index.js
package.json
README.md
LICENSE

server.js
```

**Notes:**

**env :** Config file that overrides the default per environment, you can change the entire config for your production environment

**database :** Information about your database: type, migration strategy, et cetera.

**footprints :** Options for your REST API: add a prefix, enable/disable footprint, et cetera.

**i18n :** Configuration for multi language support: set default language and translations.

**locales :** Folder in which to put all translation files

**log :** Logger options for your project. Uses winston by default.

**main :** Allow you to enable/disable trailpacks.*

**policies :** Allow you to map your policies with controllers.

**routes :** Allows you to map your controller’s functions with routes.

**session :** Options for managing sessions: strategies, cookies, web tokens, et cetera.

**views :** Allow you to configure your template engine.

**web :** Options for web server, like port.

**webpack :** Configuration for building assets, it’s a default webpack configuration (http://webpack.github.io/docs/configuration.html)

### Path: 

> [TrailsProject/](./trailsProject/README.md)
