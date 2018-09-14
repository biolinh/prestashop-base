#  Dockerized for PrestaShop

Availablity images: 

* biolinh/prestashop-base:ps-release 
* biolinh/prestashop-base:ps-5.6-fpm-apache  
* biolinh/prestashop-base:5.6-fpm-apache 
* biolinh/prestashop-base:ps1.7-dev 
* biolinh/prestashop-base:ps1.7-dev-5.6-fpm-apache 

Those images support xdebug, fpm and latest apache.

There are two resources to get the code of Prestashop

## Release page (ps-release-5.6-apache)
Prestashop's source code has been released in Prestashop Office site [Prestashop Office site](https://www.prestashop.com/en/previous-versions)
Availablity images: 
* biolinh/prestashop-base:ps-release 
* biolinh/prestashop-base:ps-5.6-fpm-apache 

## Prestashop's source code in GitHub (ps1.7-dev-5.6-apache)
From GitHub, we can get not only all released-tagged code corresponding those versions in release page but also develop branch.
We can have latest code of Prestashop but it is still in development mode. So, make your choice.

Notes: Althought you can specific any version to use, but using 1.6.1.x version, the prestashop's front-end doesn't work, only show logo, even its' back-end works propertify. I suggest using prestashop version 1.7.x.y
Availablity images: 
* biolinh/prestashop-base:5.6-fpm-apache 
* biolinh/prestashop-base:ps1.7-dev 
* biolinh/prestashop-base:ps1.7-dev-5.6-fpm-apache 

## Install which Prestashop version you need

Open the docker-compose.yml in corresponding  folder, change the value of PS_VERSION to the version you need (ex: 1.7.4.2)

# Requirements

If you are on Linux you should install

- [docker](http://docs.docker.com/compose/install/#install-docker) and
- [docker-compose (formerly known as fig)](http://docs.docker.com/compose/install/#install-compose)

If you are running on [Mac OS](https://docs.docker.com/engine/installation/mac/) or [Windows](https://docs.docker.com/engine/installation/windows/) you can install the [Docker Toolbox](https://docs.docker.com/engine/installation/mac/) which contains docker, docker-compose and docker-machine.

## Supported tags

### PrestaShop version
All of released versions in Prestashop Office site 
https://www.prestashop.com/en/previous-versions

### PHP versions
By default, our images are running with PHP 5.6 with fpm, apache and xdebug


### Apache versions

This image is running with the latest Apache version in the [official PHP repository](https://registry.hub.docker.com/_/php/).

### SQL versions
For the database, you can use and link any SQL server related to MySQL. We advise MySQL 5.6 for PrestaShop 1.6 and MySQL 5.7 for Prestashop 1.7 . MySQL 8 can be used with additional configuration.

Currently if you do not have any MySQL server, the most simple way to run this container is:
```bash
# create a network for containers to communicate
$ docker network create prestashop-net
# launch mysql 5.7 container
$ docker run -ti --name some-mysql --network prestashop-net -e MYSQL_ROOT_PASSWORD=admin -p 3307:3306 -d mysql:5.7
# launch prestashop container
$ docker run -ti --name some-prestashop --network prestashop-net -e DB_SERVER=some-mysql -p 8001:80 -d prestashop/prestashop
```

## How to run 

[![Watch the video](https://raw.github.com/GabLeRoux/WebMole/master/ressources/WebMole_Youtube_Video.png)](https://youtu.be/-eFWNe21DWM)

### Docker run

You can use tags for this. For example:
```
$ docker run -ti --name my-docker-name -e PS_DEV_MODE=false -e PS_INSTALL_AUTO=0 -p 8080:80 -d biolinh/prestashop-base:ps-release
```

### Docker-compose 
```
docker-compose up
```
### script
To start docker:
```
./prestashop start
```

To stop docker
```
./prestashop stop
```

A new shop will be built, ready to be installed.

You can then use the shop by reaching [http://localhost:8001](http://localhost:8001).

and admin site [http://localhost:8001/admin](http://localhost:8001/admin) with default use demo@prestashop.com and password  prestashop_demo

The MySQL server can be reached:
- from the host using port 3307 (example: `$ mysql -uroot -padmin -h localhost --port 3307`)
- from a container in the network using the URL `some-mysql`.

For example, when you reach the "database configuration" install step, the installer will ask for the "server database address": input `some-mysql`.

<hr>

However, if you want to customize the container execution, here are many available options:

* **PS_DEV_MODE**: The constant `_PS_MODE_DEV_` will be set at `true` *(default value: 0)*
* **PS_HOST_MODE**: The constant `_PS_HOST_MODE_` will be set at `true`. Usefull to simulate a PrestaShop Cloud environment. *(default value: 0)*
* **DB_SERVER**: If set, the external MySQL database will be used instead of the volatile internal one *(default value: localhost)*
* **DB_USER**: Override default MySQL user *(default value: root)*
* **DB_PASSWD**: Override default MySQL password *(default value: admin)*
* **DB_PREFIX**: Override default tables prefix *(default value: ps_)*
* **DB_NAME**: Override default database name *(default value: prestashop)*
* **PS_INSTALL_AUTO=1**: The installation will be executed. Useful to initialize your image faster. In some configurations, you may need to set **PS_DOMAIN** or **PS_HANDLE_DYNAMIC_DOMAIN** as well. (Please note that PrestaShop can be installed automatically from PS 1.5)
* **PS_ERASE_DB**: Only with **PS_INSTALL_AUTO=1**. Drop and create the mysql database. All previous mysql data will be lost *(default value: 0)*
* **PS_DOMAIN**: When installing automatically your shop, you can tell the shop how it will be reached. For advanced users only *(no default value)*
* **PS_LANGUAGE**: Change the default language installed with PrestaShop *(default value: en)*
* **PS_COUNTRY**: Change the default country installed with PrestaShop *(default value: GB)*
* **PS_FOLDER_ADMIN**: Change the name of the `admin` folder *(default value: admin. But will be automatically changed later)*
* **PS_FOLDER_INSTALL**: Change the name of the `install` folder *(default value: install. But must be changed anyway later)*
* **ADMIN_MAIL**: Override default admin email *(default value: demo@prestashop.com)*
* **ADMIN_PASSWD**: Override default admin password *(default value: prestashop_demo)*

If your IP / port (or domain) change between two executions of your container, you will need to modify this option:

* **PS_HANDLE_DYNAMIC_DOMAIN**: Add specific configuration to handle dynamic domain *(default value: 0)*

You can choose which prestashop version by specify in this parameter:

* **PS_VERSION**: Add specific prestashop version to install *(default value: 1.6.1.20)*

### Install which Prestashop version you need

Open the docker-compose.yml in ps-release-5.6-apache folder, change the value of PS_VERSION to the version you need (ex: 1.7.4.2)

## License

View [license information](https://www.prestashop.com/en/osl-license) for the software contained in this image.

