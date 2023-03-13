# Docker Moodle

This is a docker compose file designed to run moodle.

## Run App

Application will be served on port 80. It should be noted that a configuration script runs at startup so the container will be running but will not serve the application for a couple minutes. You can check the moodle container logs for information.

``` bash
docker-compose up -d
docker-compose down
```
