# quots
 
### Quots is a micro app that allows you to have a credit based system for resource management.

In a cluster every microservice may handle specific tasks per user. These tasks can be costly and the needs in resources may vary. Quots gives every user some credits that can be at any time used from any microservice.

![alt text](https://raw.githubusercontent.com/euclia/quots/blob/master/quots_pics/quotsmain.png)

Quots app is available as a docker container.  [I'm an inline-style link](https://hub.docker.com/r/euclia/quots)

# Credit and resource management for microservices

```
version: '3'

services:
  mongo:
    image: mongo
    container_name: "quots-db"
    ports:
      - 27017:27017
    networks:
      - quots-network
    environment:
      - MONGO_DATA_DIR=/data/db
      - MONGO_LOG_DIR=/dev/null
    volumes:
      - ./data/db:/data/db
    deploy:
        replicas: 1
        restart_policy:
          condition: unless-stopped
  quots:
    image: pantelispanka/quots
    ports:
      - 8000:8000
    environment:
      - MONGO_URL=mongo:27017
      - CREDITS=20
      - DATABASE=quotsdb
      - QUOTS_USER=QUOTS
      - QUOTS_PASSWORD=QUOTS
    depends_on:
      - mongo
    deploy:
        replicas: 1
        restart_policy:
          condition: unless-stopped

networks:
  quots-network:
    external: true
```


Env's
MONGO_URL -> The database url
CREDITS -> The default credits per user that is created
DATABASE -> The name of the database
QUOTS_USER -> Quots admin username
QUOTS_PASSWORD -> Quots admin password



Login to create applications / microservices that uses quots 


![alt text](https://raw.githubusercontent.com/euclia/quots/blob/master/quots_pics/appshowcase.png)

Add usages and costs per usage


And your micriservice is ready to use Quotas per user!

![alt text](https://raw.githubusercontent.com/euclia/quots/blob/master/quots_pics/user.png)



Usage through clients 

```
Install-Package netquots -Version 0.1.0
```
```
<dependency>
  <groupId>xyz.euclia</groupId>
  <artifactId>jquots</artifactId>
  <version>0.0.4</version>
</dependency>
```

```pip install pyquots```


Documentation soon to come 


