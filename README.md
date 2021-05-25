# Part 2
This part introduces container orchestration with docker-compose and relevant concepts such as docker network. By the end of this part you are able to:
- Run a group of containerized applications that interact with each other via HTTP
- Run a group of containerized applications that interact with each other via volumes
- Manually scale applications
- Use 3rd party services, such as databases, inside containers as part of your project

## Exercises

### 2.1
*Exercises in part 2 should be done using docker-compose*
Without a command  `devopsdockeruh/simple-web-service ` will create logs into its  `/usr/src/app/text.log`.
Create a  `docker-compose.yml ` file that starts  `evopsdockeruh/simple-web-service ` and saves the logs into your filesystem.
Submit the  `docker-compose.yml`, make sure that it works simply by running docker-compose up if the log file exists.

#### Command used:
```
% docker-compose build
% docker-compose run
```

### 2.2
Read about how to add command to docker-compose.yml from the [documentation](https://docs.docker.com/compose/compose-file/compose-file-v3/#command).
The familiar image `devopsdockeruh/simple-web-service` can be used to start a web service.
Create a docker-compose.yml and use it to start the service so that you can use it with your browser.
Submit the docker-compose.yml, make sure that it works simply by running `docker-compose up`

#### Command used:
```
% docker-compose up
```

### 2.3
**This exercise is mandatory**
As we saw previously, starting an application with two programs was not trivial and the commands got a bit long.
In the previous part we created Dockerfiles for both [frontend](https://github.com/docker-hy/material-applications/tree/main/example-frontend) and [backend](https://github.com/docker-hy/material-applications/tree/main/example-backend). Next, simplify the usage into one docker-compose.yml.
Configure the backend and frontend from part 1 to work in docker-compose.
Submit the docker-compose.yml

#### Command used:
```
% docker-compose up
```

### 2.4
Add redis to example backend.
Redis is used to speed up some operations. Backend uses a slow api to get information. You can test the slow api by requesting /`ping?redis=true` with curl. The frontend program has a button to test this.
Configure a redis container to cache information for the backend. Use the documentation if needed when configuring: https://hub.docker.com/_/redis/
The backend [README](https://github.com/docker-hy/material-applications/tree/main/example-backend) should have all the information needed to connect.
When you’ve correctly configured the button will turn green.
Submit the docker-compose.yml
![](https://docker-hy.github.io/images/exercises/back-front-and-redis.png)

> `restart: unless-stopped` can help if the redis takes a while to get ready
> TIP: If you’re stuck check out [tips and tricks](https://devopswithdocker.com/exercise_tricks)

#### Command used:
```
% docker-compose up
```

