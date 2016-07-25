# Docker Symfony (PHP7-FPM - NGINX - MySQL - ELK - REDIS)


## Config:

1. Modify docker-compose.yml with symfony path

    ```yml
    services:
        php:
            volumes:
                - path/to/your/symfony-project:/var/www/symfony
    ```

2. Build containers (with and without detatched mode)
    
    ```bash
    $ docker-compose up
    $ docker-compose up -d
    ```

    **Note:** If you have any service running on ports defined on docker-compose.yml, build will fail. Stop your apache, mysql (..) services before you run your dockers.

3. Update hosts file

    ```bash
    $ sudo echo "127.0.0.1 symfony.dev" >> /etc/hosts
    ```

4. Modify symfony

    1. Update app/config/parameters.yml

        ```yml
        # path/to/sfApp/app/config/parameters.yml
        parameters:
            redis_host: redis
            database_host: mysqldb
        ```

    2. Composer install & create database

5. Enjoy :-)

## Usage

Just run `docker-compose -d`, then:

* Symfony app: visit [symfony.dev](http://symfony.dev)  
* Symfony dev mode: visit [symfony.dev/app_dev.php](http://symfony.dev/app_dev.php)  
* Logs (Kibana): [symfony.dev:81](http://symfony.dev:81)
* Logs (files location): logs/nginx and logs/symfony
* Phpmyadmin: [symfony.dev:8000](http://symfony.dev:8080))

## Useful commands

```bash
# bash commands
$ docker-compose exec php bash

# Composer (e.g. composer update)
$ docker-compose exec php composer update

# SF commands (Tips: there is an alias inside php container)
$ docker-compose exec php php /var/www/symfony/app/console cache:clear # Symfony2
$ docker-compose exec php php /var/www/symfony/bin/console cache:clear # Symfony3
# Same command by using alias
$ docker-compose exec php bash
$ sf cache:clear

# MySQL commands
$ docker-compose exec db mysql -uroot -p"root"

# Redis commands
$ docker-compose exec redis redis-cli

# F***ing cache/logs folder
$ sudo chmod -R 777 app/cache app/logs # Symfony2
$ sudo chmod -R 777 var/cache var/logs # Symfony3

# Check CPU consumption
$ docker stats $(docker inspect -f "{{ .Name }}" $(docker ps -q))

# Delete all containers
$ docker rm $(docker ps -aq)

# Delete all images
$ docker rmi $(docker images -q)
```


## Notes

Inspired in

    https://github.com/maxpou/docker-symfony
    https://github.com/eko/docker-symfony
