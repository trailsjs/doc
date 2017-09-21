# Docker Clustering

If you have your api packaged in a docker container, you can use an orchestration platform to scale the application.

This example assumes you setup docker in the manner specified in [the trails Docker tutorial](docker.md).

## [Docker Swarm](https://docs.docker.com/engine/swarm/)

_docker-compose.yml_
```yml
version: '3'

services:
  some-trails:
    image: trailsjs/trails:latest
    ports:
      - 80:3000
    links:
      - some-mongo:db

  some-mongo:
    image: mongo:latest
    volumes:
      - /volumes/trails_some-trails:/data/db
```

```sh
docker stack deploy --compose-file ./docker-compose.yml trails
docker service scale trails_some-trails=3
```

#### [Rancher](http://rancher.com/)

##### Without Internal Load Balancer

_docker-compose.yml_
```yml
some-trails:
  image: trailsjs/trails:latest
  ports:
    - 80:3000
  links:
    - some-mongo:db
    
some-mongo:
  image: mongo:latest
  volumes:
    - /volumes/trails_some-trails:/data/db
```
_rancher-compose.yml_
```yml
some-trails:
  scale: 3
```

##### With Internal HaProxy Load Balancer

_docker-compose.yml_
```yml
some-trails:
  image: trailsjs/trails:latest
  links:
    - some-mongo:db
    
some-mongo:
  image: mongo:latest
  volumes:
    - /volumes/trails_some-trails:/data/db
    
load-balancer:
  image: rancher/lb-service-haproxy:v0.7.1
  ports:
    - 80:80/tcp
  labels:
    io.rancher.container.agent.role: environmentAdmin
    io.rancher.container.create_agent: 'true'
```
_rancher-compose.yml_
```yml
some-trails:
  scale: 3
load-balancer:
  start_on_create: true
  lb_config:
    port_rules:
    - hostname: trails.example.com
      path: ''
      protocol: http
      service: trailblazer/some-trails
      source_port: 80
      target_port: 3000
```

#### [Kubernetes](https://kubernetes.io/)

#### [Mesos](http://mesos.apache.org/)
