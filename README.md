# Pimcore Sample

Simple Dockerized Environment for Pimcore.<br>
This project is based on [Pimcore Skeleton](https://github.com/pimcore/skeleton).

## Getting Started

### Requirements

* [Docker](https://www.docker.com/)
* [Composer](https://getcomposer.org/)

Tip: run the following for Mac user to install Composer

```bash
brew install composer
```

### Setup (For Initial Run)

```bash
# install PHP dependencies
composer install --ignore-platform-reqs
# start docker compose
docker-compose up -d
# run pimcore installer
docker-compose run --rm php vendor/bin/pimcore-install --mysql-host-socket db \
--admin-username admin --admin-password admin \
--mysql-username pimcore --mysql-password pimcore \
--mysql-database pimcore --no-interaction
# fix permission
docker-compose run --rm php chown -R www-data:www-data var/*
```

Access respectively:

- Pimcore: [http://127.0.0.1](http://127.0.0.1)
- Pimcore Admin: [http://127.0.0.1/admin](http://127.0.0.1/admin)
- Adminer: [http://127.0.0.1:8080](http://127.0.0.1:8080)

### Stop Container

```bash
# pause: keep all data
docker-compose stop
```

### Start Container

```bash
# start from pause
docker-compose start
```

## Tips

### Pimcore Project Structure

```
pimcore-demo
+- app
|  +- config
|  |  +- local
|  |  |  +- database.yml <--- DB Info
|  |  +- ...
|  |  +- config.yml <--- App Config
|  |  +- ...
|  +- Resources
|  |  +- views
|  |     +- Default
|  |        +- default.html.php <--- Default view
|  +- ...
+- bin
+- src
|  +- AppBundle
|     +- ...
|     +- Controller
|     |  +- DefaultController.php <--- Default controller
|     +- ...
+- var
|  +- ...
|  +- classes
|     +- ...
|     +- definition_[YOUR_CLASS].php <--- Class definition
|  +- logs
|     +- dev.log
+- vendor
+- web <--- Root for Apache or NGINX
+- .editorconfig
+- .env.example
+- .gitattributes
+- .gitignore
+- .php_cs.dist
+- composer.json
+- composer.lock
+- docker-compose.yml
+- gpl-3.0.txt
+- README.md
```

### Start Pimcore Project from Scratch

Pimcore offers 4 different installation packages, **3 demo packages** and **one skeleton** for experienced developers. Check the link below:<br>
https://pimcore.com/docs/6.x/Development_Documentation/Getting_Started/Installation.html#page_Choose-a-package-to-install

1. Remove all files except for the following

    - README.md
    - .git
    - .gitignore
    - .gitattributes

2. Download Pimcore package

    ```bash
    COMPOSER_MEMORY_LIMIT=-1 composer create-project pimcore/skeleton tmp --ignore-platform-reqs
    ```

3. Move Pimcore project 1 hierachy up

    ```bash
    rm -rf tmp/README.md tmp/.gitignore tmp/.gitattributes
    mv tmp/*(DN) ./
    rm -rf tmp
    ```

4. Run Installer

    ```bash
    # Run Docker
    docker-compose up -d
    # pimcore install
    docker-compose run --rm php vendor/bin/pimcore-install --mysql-host-socket db \
    --admin-username admin --admin-password admin \
    --mysql-username pimcore --mysql-password pimcore \
    --mysql-database pimcore --no-interaction
    # fix permission
    docker-compose run --rm php chown -R www-data:www-data var/*
    ```

6. Check on Brower at `http://127.0.0.1/admin` with admin credential you set up right above `--admin-username` and `--admin-password`

### Access to Pimcore Docker Image

```bash
docker exec -it pimcore-php bash
```

### Access to DB

1. Go into docker container

    ```bash
    docker exec -it pimcore-php bash
    ```

2. Login to DB (no space after -p)

    ```bash
    mysql -u pimcore -p'pimcore' -h database -P 3306 -D pimcore
    ```

### Useful Docker Commands

```bash
# removes containers and default network, but preserves your Pimcore database
docker-compose down
# removes containers, default network, and Pimcore database
docker-compose down --volumes
# check volumes
docker volume ls
# remove all unused volumes
docker volume prune
```
