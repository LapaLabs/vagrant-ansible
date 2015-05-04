# Vagrant provisioning using Ansible playbook roles

The Symfony Demo Application example of vagrant provisioning using various separate Ansible playbook roles based on Ubuntu 14.04 box for Parallels virtualization.

## Precondition

Need to install a few Vagrant plugins before to use `vagrant up`:

* [Vagrant Parallels Provider](http://parallels.github.io/vagrant-parallels/)
* [Vagrant Hostmanager](https://github.com/smdahlen/vagrant-hostmanager)

## Role List

* init
* php5
* php5-cli
* php5-fpm
* nginx
* mysql
* npm
* symfony-installer
* composer
* phpmyadmin
* phpunit
* app
* linux-easter-eggs

> You can easily exclude every role from vagrant provision, just comment out it in `ansible/playbook.yml`.

## Default Variables

Some roles use predefined variables, which helps you easily configure it.
You should to define them before to do provision.
There is example of provision [Symfony Demo Application](https://github.com/symfony/symfony-demo) environment using Ansible playbook.

### Init

``` yaml
timezone: Europe/Kiev
init_packages:
    - curl
    - wget
    - git
```

### PHP

``` yaml
php5_ppa: ppa:ondrej/php5-5.6
php5_timezone: Europe/Kiev
php5_display_errors: "On"
php5_error_reporting: E_ALL
php5_mbstring_func_overload: 0
php5_xdebug_max_nesting_level: 250
php5_post_max_size: 32M
php5_upload_max_filesize: 32M
php5_packages:
    # php5 - will be installed by default
    # php5-fpm - will be installed by default with php5-fpm role
    # php5-cli - will be installed by default with php5-cli role
    - php5-sqlite # used by Symfony Demo App as default data storage
    - php5-mysql # could be used by Symfony Demo App as alternative data storage
    - php5-intl # required by Symfony
    - php5-xdebug # optional, used for convenience debug
```

### Nginx

``` yaml
nginx_packages: []
    # nginx - will be installed by default
nginx_sites:
    symfony-demo:
        source_config_template: symfony-standard # Nginx template name for Symfony Framework
        server_name: demo.symfony.localhost www.demo.symfony.localhost
        root: /var/www/html/web
        index: app_dev.php app.php
        default_front_controller_name: app_dev
```

The `nginx` role also provide multiple virtual hosts creation, based on next templates:

* php
* symfony-standard
* phpmyadmin

### MySQL

``` yaml
mysql_packages:
    # mysql-server - will be installed by default
    - python-mysqldb # required to create database
mysql_databases:
    symfony-demo:
        name: symfony-demo
        user: user
        password: userpass
```

The `mysql` role also provide multiple databases creation, each with its own separate user and password.

### npm

``` yaml
npm_packages:
    # npm - will be installed by default
    - bower
    - uglify-js
    - uglifycss
```

### App

``` yaml
app_packages: []
```

### Linux Easter Eggs

``` yaml
linux_easter_eggs_packages:
    - sl
    - cowsay
    - fortunes
```
