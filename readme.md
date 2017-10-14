## Докер-образ `docker-php71-pg-redis`

[![MASTER build](https://img.shields.io/docker/build/avtodev/docker-php71-pg-redis.svg)](https://hub.docker.com/r/avtodev/docker-php71-pg-redis)
[![GitHub issues](https://img.shields.io/github/issues/avto-dev/docker-php71-pg-redis.svg)](https://github.com/avto-dev/docker-php71-pg-redis/issues)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/avtoraport/docker-php71-pg-redis/master/license)

Данный образ включает в себя:

 * `php 7.1`;
 * `composer`;
 * `postgres 9.6`;
 * `redis 3.0.6`;
 * `supervisor`;
 * `deployer`;
 * `git & ssh`.

Для запуска демонов `postgres` и `redis` используется `supervisor`.

### Docker HUB

[/r/avtodev/docker-php71-pg-redis][docker_hub]

### Назначение

Данный образ используется как базовый для запуска `php` приложений. Если вам необходимо дополнить его, нарпимер, `nginx + php-fpm`-ом, то вам достаточно указать его как базовый, дописать необходимые шаги (при необходимости добавления демонов, например `php-fpm` - просто допилите конфиг его запуска с помощью `supervisor` и положите его в директорию `/etc/supervisor/conf.d/` с помощью директивы `ADD`).

Для его использования с помощью `gitlab ci` необходимо запускать `supervisor` ручками, например так:

```yml
# GitLab CI help: <https://docs.gitlab.com/ee/ci/yaml/>

# Docker image page: <https://hub.docker.com/r/avtodev/docker-php71-pg-redis>
image: avtodev/docker-php71-pg-redis

variables:
  GIT_STRATEGY: clone

stages:
  - build
  - deploy

before_script:
  - echo "> Starting supervisor.."
  - /etc/init.d/supervisor start &
  - until runuser -l postgres -c 'pg_isready' 1>/dev/null 2>&1; do echo 'Wait for daemon starts..'; sleep 1; done;
  
# ...
```

### Лицензирование

Код данного репозитория распространяется под лицензией **MIT**.

[docker_hub]:https://hub.docker.com/r/avtodev/docker-php71-pg-redis/
