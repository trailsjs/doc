# Anatomy
###### Trails Anatomy

## Tags
```config directory```

Environment config files are used to override default configuration files depending on the NODE_ENV value. 
For example if NODE_ENV is set to `testing` then the file `env/testing.js` will be load and merge with de default configuration files.
 
It can be useful to set specific configuration like database, different log level on production and development environment.