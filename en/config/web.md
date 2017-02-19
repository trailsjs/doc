#### [Docs](../../) / [Configuration](./) / web.js

# 3.7. `web.js`

Configure the web server. Set the listening port, load any plugins, and define advanced web server settings. Trails supports several web servers, so the particular configuration options available will be provided by the chosen web server Trailpack (e.g., express, hapi).

```js
// config/web.js

module.exports = {
  /**
   * The port to bind the web server to
   */
  port: process.env.PORT || 3000,

  /**
   * The host to bind the web server to
   */
  host: process.env.HOST || '0.0.0.0',

  // ... etc
}
```

## Hapi

The [Hapi Trailpack](https://github.com/trailsjs/trailpack-hapi) provides the ability to configure the [Hapi](https://hapijs.com/) web server in Trails.

```js
// config/web.js

module.exports = {
  // ...

  /**
   * Hapi web server plugins
   */
  plugins: [
    {
      register: require('vision'),
      options: { }
    },
    {
      register: require('inert'),
      options: { }
    },

    // ... other plugins
  ],

  /**
   * Method invoked once the Hapi plugins have successfully loaded to complete
   * any necessary initialization. Here, we're setting up Hapi for server-side
   * rendering of a React.js application.
   *
   * @this refers to the Trails application object
   */
  onPluginsLoaded (err) {
    this.packs.hapi.server.views({
      engines: {
        js: require('hapi-react-views')
      },
      relativeTo: this.config.get('main.paths.root'),
      path: 'dist',
      compileOptions: {
        renderMethod: 'renderToString',
        layoutPath: path.resolve(this.config.get('main.paths.root'), 'dist'),
        layout: 'html'
      }
    })
  }
}
```

## Express

The [Express Trailpack](https://github.com/trailsjs/trailpack-express) provides the ability to configure the [Express](http://expressjs.com) web server in Trails.

```js
// config/web.js

module.exports = {

  /**
   * CORS options
   * Can be true/false or an object of CORS options
   * @see {@link https://github.com/expressjs/cors#configuring-cors}
   */
  cors: false,

	middlewares: {
		order: [
      // ... load custom middleware
    ],

    /**
     * Custom request body parser
     */
    bodyParser: [
      bodyParser.json()
    ]
  },

  redirectToHttps: false,
  /**
   * The number of seconds to cache flat files on disk being served by
   * Express static middleware (by default, these files are in `.tmp/public`)
   *
   * The HTTP static cache is only active in a 'production' environment,
   * since that's the only time Express will cache flat-files.
   */
  cache: 60 * 60 * 24 * 365,

  // ...
}
```
