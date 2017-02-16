# Start

Trails is a framework, and is downloadable as a module from [**npm**](https://www.npmjs.com/package/trails). This guide will walk through how to set up a new Trails application using the Trails installer.

If you haven't yet installed Node, download it from [**nodejs.org**](https://nodejs.org/en/) and follow the instructions there.

## Install Trails

Trails includes an interactive installer that guides us through the process of setting up a new app. First, download the installer using npm:

#### `npm install -g yo generator-trails`

## Create a New App

Now, create a new directory for your application and navigate to that directory in your console. For example, in Mac/Linux:

```bash
$ mkdir helloworld
$ cd helloworld
```

In your console, start the interactive installer by running:

#### `yo trails`

*If you're in Windows you may have to add the npm install directory to the `PATH` environment variable. e.g. `C:\Users\username\AppData\Roaming\npm`. See also: [http://stackoverflow.com/a/32159233/291180](http://stackoverflow.com/a/32159233/291180).

#### 1. Choose web Server

Default: `hapi`.

Trails supports many of the popular Node.js web server modules. If this is your first Trails application, we recommend starting with either Hapi or Express.

```
Get ready to blaze a new Trails Application!

Checking for updates...
? Choose a Web Server (Use arrow keys)
❯ hapi
  express
  other
```

#### 2. Choose ORM

Default: `waterline`

An ORM (object-relational mapping) can be included with your new Trails application. Like all plugins, they can easily be added later.

```
? Choose an ORM (Use arrow keys)
❯ waterline
  mongoose
  knex
  sequelize
  bookshelf
  none
  other
```


#### 3. Use Footprints?

Default: `yes`

Trails can automatically generate RESTful endpoints based on the Models you define using a plugin called Footprints. This is a great way to speed up development of your project while requirements are in flux; you can always reduce the API surface area by disabling footprints and explicitly configuring routes.

```
? Do you want to use Footprints (automatic REST API from models) ? (Y/n)
```

#### 4. Module Information

Specify the name, description, and other attributes of your new module. These values will be set in the module's `package.json`.

```
? Module Name (new-trails-app)
? Description this is a new trails app
? Project homepage url trailsjs.io
? Author's Name trailsjs
? Author's Email hello@trailsjs.io
? Author's Homepage trailsjs.io
? Package keywords (comma to split) trails, quickstart, documentation
```

#### 5. License

Default: `MIT`

```
? Which license do you want to use?
❯ MIT
  Apache 2.0
  Unlicense
  FreeBSD
  NewBSD
  Internet Systems Consortium (ISC)
  No License (Copyrighted)
```

#### 6. Done!

A bunch of new files will be created, and you'll be able to fire up your new Trails application and start building features.

```
┌───────────────────────────────────────────┐
│ Your Trails Application has been created! │
│ ---                                       │
│ To start your application, run: npm start │
└───────────────────────────────────────────┘
```

## Next: [Build your App](./build/README.md)
