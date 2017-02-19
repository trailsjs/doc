# 3.2. `main.js`

Declare the Trailpacks to load, set filesystem paths, and other basic application settings.

```js
// config/main.js

module.exports = {
  packs: [
    require('trailpack-router'),
    require('trailpack-hapi'),
    require('trailpack-mapnik')
  ],

  paths: {
    root: path.resolve(__dirname, '..'),
    // ... other paths
  }
}
```

## `main.packs`

The list of Trailpacks to load into the framework. Trailpacks to include can be found on the [Plugins](http://trailsjs.io/plugins) page. Note: if `main.packs` is empty, the program will start and immediately exit!

As discussed in the [Trails Design Theory](../ref/theory.md) section:

> The Trails framework has exactly one feature: the ability to harmoniously manage Trailpacks.
> The capability available to the developer is thus an emergent property of the Trailpacks the user has installed. 

Read more about Trailpacks:
- [Concepts: Trailpack](../build/trailpack.md)
- [Reference: Trailpack API](../ref/trailpack.md)

## `main.paths`

Trails and Trailpacks use these paths during runtime for many reasons. Trails and some Trailpacks may store temporary information such as REPL history, compiled asset files, logfiles, etc.
