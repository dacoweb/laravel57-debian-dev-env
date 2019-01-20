# PHP 7.2 Debian Dev Environment for Laravel 5.7

Template for development environment.

## Develpment Environment:

* Mysql 5.7
* PHP7.2 with phpfpm (official debian:stretch-slim container: php:7.2-fpm)
* XDebug for PHP

## Requirements

Requirements to run this project:

- [Docker version 18.06.1-ce+](https://github.com/docker/docker-ce)
- [docker-compose version 1.19.0+](https://github.com/docker/compose) 


## Getting Started

Clone this repo into any local directory as normal... (git clone...), then:

Build containers:

```
$ docker-compose build
```

Run containers on detached mode:

```
$ docker-compose up -d
```

Create a laravel project

```
$ docker exec -it --user www-data {php_container} bash
$ composer create-project --prefer-dist laravel/laravel application
```

Make sure to update the laravel environment file. (env) to point to db container

```
...
DB_HOST=db
...
```

Finally the php application is expected to be located on the `src-code/application` directory.

Open in browser with:

http://localhost:8080/


## Run composer, php artisan commands or tests with devtool:

```
./devtool artisan
./devtool composer
./devtool test
...
```

Example: To generate the default laravel authentication feature, run:

```
./devtool artisan make:auth
```

## Debugging Process (Set to work with Visual Studio Code)

After clone this project, open with VSCODE, set a breackpoint and hit F5. This should be enough start debuging the source code (src-code).

Setting for VSCODE should be located on the .vscode directory, and should look as follow:

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9100,
            "pathMappings": {
                "/var/www/html/application/": "${workspaceFolder}/src-code/application/"
            }
        },
    ]
}
```

* **David Castillo** - *Web Developer* - [dacoweb](https://github.com/dacoweb)
