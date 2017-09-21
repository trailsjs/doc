This example assuming you are using a the follwing database configuration
_config/database.js_
```js
module.exports = {
  stores: {
    mongo: {
      adapter: require('sails-mongo'),
      host: process.env.MONGO_HOST ? process.env.MONGO_HOST : 'localhost',
      port: process.env.MONGO_PORT ? process.env.MONGO_PORT : 27017,
      database: process.env.MONGO_DATABASE ? process.env.MONGO_DATABASE : 'trails'
    }
  },

  models: {
    defaultStore: 'mongo',
    migrate: 'safe'
  }
};
```

## 1. Create a docker file in the root of your project

  _Dockerfile_
  ```docker
  #############################################
  # Dockerfile to run trailsjs/trails:latest
  # Based on Alpine
  #############################################

  FROM node:7.8-alpine

  MAINTAINER Jam Risser (jamrizzi)

  ENV MONGO_HOST=db
  ENV MONGO_PORT=27017
  ENV MONGO_DATABASE=trails
  ENV NODE_ENV=production

  EXPOSE 3000

  WORKDIR /app/

  RUN apk add --no-cache \
          tini && \
      apk add --no-cache --virtual build-deps \
          git

  COPY ./package.json /app/
  RUN npm install
  COPY ./ /app/
  RUN apk del build-deps && \
      npm prune --production

  ENTRYPOINT ["/sbin/tini", "--", "node", "/app/server.js"]
  ```

## 2. Add the following scripts to your `package.json`

  ```json
  {
   "scripts": {
      "build": "docker build -t trailsjs/trails:latest -f ./Dockerfile .",
      "push": "docker push -t trailsjs/trails:latest",
      "run": "docker run --name some-trails --rm --link some-mongo:db -p 3000:3000 trailsjs/trails:latest"
    }
  }
  ```

## 3. Build your container

  ```sh
  npm run build
  ```

4. Push your container

  ```sh
  npm run push
  ```
