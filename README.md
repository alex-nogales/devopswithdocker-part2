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

> Command used:
>>```% docker-compose build
% docker-compose run
```
