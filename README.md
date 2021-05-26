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

### 2.5
A project over at https://github.com/docker-hy/material-applications/tree/main/scaling-exercise has a hardly working application. Go ahead and clone it for yourself. The project already includes docker-compose.yml so you can start it by running `docker-compose up`.
Application should be accessible through http://localhost:3000. However it doesn’t work well enough and I’ve added a load balancer for scaling. Your task is to scale the `compute` containers so that the button in the application turns green.
This exercise was created with [Sasu Mäkinen](https://github.com/sasumaki)
Please return the used commands for this exercise.

#### Command used:
```
% docker-compose up -d --scale compute=3
```

### 2.6
Add database to example backend.
Lets use a postgres database to save messages. We won’t need to configure a volume since the official postgres image sets a default volume for us. Lets use the postgres image documentation to our advantage when configuring: https://hub.docker.com/_/postgres/. Especially part Environment Variables is of interest.
The backend [README](https://github.com/docker-hy/material-applications/tree/main/example-backend) should have all the information needed to connect.
The button won’t turn green but you can send messages to yourself.
Submit the docker-compose.yml
> TIP: When configuring the database, you might need to destroy the automatically created volumes. Use command `docker volume prune`, `docker volume ls` and `docker volume rm` to remove unused volumes when testing. Make sure to remove containers that depend on them beforehand.
> `restart: unless-stopped` can help if the postgres takes a while to get ready

![](https://docker-hy.github.io/images/exercises/back-front-redis-and-database.png)

çç

### 2.7
Configure a [machine learning](https://en.wikipedia.org/wiki/Machine_learning) project.
Look into machine learning project created with Python and React and split into three parts: [frontend](https://github.com/docker-hy/ml-kurkkumopo-frontend), [backend](https://github.com/docker-hy/ml-kurkkumopo-backend) and [training](https://github.com/docker-hy/ml-kurkkumopo-training)
Note that the training requires 2 volumes and backend should share volume `/src/model` with training.
The frontend will display on http://localhost:3000 and the application will tell if the subject of an image looks more like a cucumber or a moped.
Submit the docker-compose.yml
> This exercise is known to have broken for some attendees based on CPU. The error looks something like “Illegal instruction (core dumped)”. Try downgrading / upgrading the tensorflow found in requirements.txt or join the telegram channel and message with @jakousa.
> Note that the generated model is a toy and will not produce good results.
> It will take SEVERAL minutes to build the docker images, download training pictures and train the classifying model.
This exercise was created by [Sasu Mäkinen](https://github.com/sasumaki)

>> Can't do this exercise because of an error when building the image: `ERROR: Could not find a version that satisfies the requirement tensorflow==1.15 (from versions: none)`

### 2.8
Add nginx to example frontend + backend.
![](https://docker-hy.github.io/images/exercises/back-front-redis-database-and-nginx.png)
Accessing your service from arbitrary port is counter intuitive since browsers use 80 (http) and 443 (https) by default. And having the service refer to two origins in a case where there’s only one backend isn’t desirable either. We will skip the SSL setup for https/443.
Nginx will function as a reverse proxy for us (see the image above). The requests arriving at anything other than /api will be redirected to frontend container and /api will get redirected to backend container.
At the end you should see that the frontend is accessible simply by going to http://localhost and the button works. Other buttons may have stopped working, do not worry about them.
As we will not start configuring reverse proxies on this course you can have a simple config file:
The following file should be set to /etc/nginx/nginx.conf inside the nginx container. You can use a file volume where the contents of the file are the following:
```
  events { worker_connections 1024; }

  http {
    server {
      listen 80;

      location / {
        proxy_pass _frontend-connection-url_;
      }

      location /api/ {
        proxy_set_header Host $host;
        proxy_pass _backend-connection-url_;
      }
    }
  }
```
Nginx, backend and frontend should be connected in the same network. See the image above for how the services are connected.

Submit the docker-compose.yml

> Tips for making sure the backend connection works:
> Try using your browser to access http://localhost/api/ping and see if it answers pong
> It might be nginx configuration problem: Add trailing / to the backend url in the nginx.conf.

#### Command used:
```
% docker-compose up
```

### 2.9
Postgres image uses a volume by default. Manually define volumes for the database in convenient location such as in `./database` . Use the image documentations (postgres) to help you with the task. You may do the same for redis as well.
After you have configured the volume:
Save a few messages through the frontend
Run `docker-compose down`
Run `docker-compose up` and see that the messages are available after refreshing browser
Run `docker-compose down` and delete the volume folder manually
Run `docker-compose` up and the data should be gone
Maybe it would be simpler to back them up now that you know where they are.
> TIP: To save you the trouble of testing all of those steps, just look into the folder before trying the steps. If it’s empty after docker-compose up then something is wrong.
> TIP: Since you may have broken the buttons in nginx exercise you should test with docker-compose.yml from before it
Submit the docker-compose.yml


>> *Note: Had to delete the nginx container to make the buttons work* 
#### Command used:
```
% docker-compose up
```