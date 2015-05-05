# Vagrant provisioning using Ansible

The [Symfony Demo Application](https://github.com/symfony/symfony-demo) example of `Vagrant` provisioning using various separate `Ansible` playbook roles based on `Ubuntu 14.04` box for `Parallels` or `VirtualBox` virtualization tool which is easy to extend.

## Precondition

You need to install a few `Vagrant` plugins before to use `vagrant up`:

* [Vagrant Parallels Provider](http://parallels.github.io/vagrant-parallels/) - if you prefer `Parallels` VM instead of `VirtualBox` one by default.
* [Vagrant Hostmanager](https://github.com/smdahlen/vagrant-hostmanager) - to manage the `/etc/hosts` files automatically.

## Role List

* init
* nginx
* mysql
* php5
* php5-cli
* php5-fpm
* npm
* symfony-installer
* composer
* phpmyadmin
* phpunit
* app
* linux-easter-eggs

> You can easily to add your own additional roles or to exclude some existent roles from provision, just comment out it in `ansible/playbook.yml` file.

## Default Variables

Some roles use predefined variables, which helps you easily configure it.
You should to define them before to do provision.

### Init

``` yaml
timezone: Europe/Kiev
init_packages:
    - curl
    - wget
    - git
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

### PHP

``` yaml
php5_ppa: ppa:ondrej/php5-5.6
php5_timezone: Europe/Kiev
php5_display_errors: "On"
php5_error_reporting: E_ALL
php5_mbstring_func_overload: 0
php5_xdebug_max_nesting_level: 250
php5_post_max_size: 8M
php5_upload_max_filesize: 2M
php5_packages:
    # php5 - will be installed by default
    # php5-fpm - will be installed by default with php5-fpm role
    # php5-cli - will be installed by default with php5-cli role
    - php5-sqlite # used by Symfony Demo App as default data storage
    - php5-mysql # could be used by Symfony Demo App as alternative data storage
    - php5-intl # required by Symfony
    - php5-xdebug # optional, used for convenience debug
```

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
