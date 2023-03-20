# Docker Moodle

This is a docker compose file designed to run moodle.

## Run App

Application will be served on port 80. It should be noted that moodle runs several setup & configuration script at first startup so the container will be running but will not serve the application for a couple minutes. You can check the moodle container logs for information on the setup process.

``` bash
cp .env.dev.example .env.dev
cp .env.staging.example .env.staging
cp .env.prod.example .env.prod

docker-compose --env-file .env.dev up -d --build    # For development
docker-compose --env-file .env.staging up -d --build    # For staging
docker-compose --env-file .env.prod up -d --build   # For Production

docker-compose down
```

Due to the fact that the bitnami containers are mostly non-root you need to alter the configuration or simply change the permissions of the docker directory.

``` bash
sudo chown -R 1001:1001 [dir] # /root/docker-moodle
```

## Generate SSL Cert

``` bash
# Generate SSL certificate for your domain Via certbot. Add ```--dry-run``` to the end of the command to test first.
docker exec -i docker-moodle_maintenance_1 certbot certonly --webroot --webroot-path /var/certbot/ -d <domain>

# Refresh httpd
docker exec -i docker-moodle_moodle_1 httpd -k restart
```

## Reset App

``` bash
rm -rf ./docker/volumes
```

## Backup App

``` bash
docker run --rm -v /path/to/moodle-backups:/backups --volumes-from moodle busybox cp -a /bitnami/moodle /backups/latest
```
