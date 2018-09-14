FROM biolinh/base:php-fpm-apache
MAINTAINER linh@jmango360.com

ENV PS_DOMAIN="<to be defined>" \
DB_SERVER="<to be defined>" \
DB_PORT=3306 \
DB_NAME=prestashop \
DB_USER=root \
DB_PASSWD=admin \
DB_PREFIX=ps_ \
ADMIN_MAIL=demo@prestashop.com \
ADMIN_PASSWD=prestashop_demo \
PS_LANGUAGE=en \
PS_COUNTRY=GB \
PS_INSTALL_AUTO=0 \
PS_ERASE_DB=0 \
PS_DEV_MODE=0 \
PS_HOST_MODE=0 \
PS_HANDLE_DYNAMIC_DOMAIN=0 \
PS_FOLDER_ADMIN=admin \
PS_FOLDER_INSTALL=install \
PS_VERSION=develop

RUN apt-get update \                                                                                                               
    && apt-get -y  install ca-certificates wget curl nano git unzip \   
				libmcrypt-dev \
				libjpeg62-turbo-dev \
				libpcre3-dev \
				libpng-dev \
				libfreetype6-dev \
				libxml2-dev \
				libicu-dev \
				mysql-client \			                                                                                          
    && update-ca-certificates \
    && rm -rf /var/lib/apt/lists/* 

ENV LD_RUN_PATH="/usr/local/lib"
ENV LD_LIBRARY_PATH="/usr/local/lib/"

RUN docker-php-ext-install mysqli      \
    && docker-php-ext-install iconv  pdo_mysql \
    && docker-php-ext-install  bcmath   opcache 
RUN apt-get update &&  apt-get install -y zlib1g-dev  libxslt-dev \
    && docker-php-ext-install zip  xsl

RUN apt-get -y install libxml2-dev \
    && docker-php-ext-configure intl --enable-intl \
    && docker-php-ext-configure soap --enable-soap  \
    && docker-php-ext-install intl 		\
    && docker-php-ext-install soap \
	&& ( \
		apt-get -y install \
			build-essential \
			pkg-config \
			libffi-dev \
			libmagickwand-dev \
			python-cffi \
		&&	docker-php-ext-configure gd \
			--with-gd \
			--with-freetype-dir=/usr/include/ \
			--with-png-dir=/usr/include/ \
			--with-jpeg-dir=/usr/include/ \
		&& docker-php-ext-install gd \
	) \
	&& ( \
		apt-get -y install libmcrypt-dev   \
		&& docker-php-ext-configure mcrypt --with-mcrypt \
		&& docker-php-ext-install  mcrypt \
	) 

#RUN apt-get -y install   libxslt-dev &&  docker-php-ext-install xsl  

# install Xdebug

RUN apt-get -y install  $PHPIZE_DEPS  autoconf \
	&& pecl install xdebug-2.5.0 \
	&& docker-php-ext-enable xdebug \
	&& echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_mode=req" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.idekey=DBGP_IDEKEY" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=127.0.0.1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini 

VOLUME /var/www/html

# Apache configuration
RUN if [ -x "$(command -v apache2-foreground)" ]; then a2enmod rewrite proxy proxy_ajp  ssl proxy_html proxy_http proxy_balancer proxy_fcgi; service apache2 restart; fi

# Prepare install and CMD script
COPY config_files/ps-extractor.sh config_files/docker_run.sh /tmp/

# If handle dynamic domain
COPY config_files/docker_updt_ps_domains.php /tmp/

# PHP configuration
COPY config_files/php.ini /usr/local/etc/php/

COPY config_files/docker_run_git.sh /tmp/

RUN ["chmod", "+x", "/tmp/docker_run_git.sh", "/tmp/docker_run_git.sh"]

# Run
# CMD ["/tmp/docker_run.sh"]
ENTRYPOINT ["/tmp/docker_run_git.sh"]

