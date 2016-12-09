# Very simple PHPUnit docker file
#

FROM phusion/baseimage

RUN set -xe

# RUN printf "deb http://packages.dotdeb.org jessie all\ndeb-src http://packages.dotdeb.org jessie all\n" \
#        >> /etc/apt/sources.list.d/extras.list

# Environment Variables
ENV DEBIAN_FRONTEND noninteractive
ENV PHP_INI_DIR /etc/php7
ENV PHP_USER www-data
ENV ENV_TYPE development

# Add the PHP7 repo
RUN curl -s https://www.dotdeb.org/dotdeb.gpg | apt-key add -


# Install PHP 7
#RUN add-apt-repository -y ppa:ondrej/php
RUN for typ in deb "deb-src"; \
    do printf "${typ} http://ppa.launchpad.net/ondrej/php/ubuntu $(lsb_release -cs) main\n" \
        >> /etc/apt/sources.list.d/ondrej-ubuntu-php-xenial.list; done

RUN apt-get update -y

RUN apt-get install -y \
    git wget zip mysql-client bash \
    libjpeg62-dev \
    libmcrypt-dev libpng12-dev \
    libicu-dev

# PHP7
RUN  apt-get install -y --allow-unauthenticated \
                php7.0 \
                php-igbinary \
                php-imagick \
                php7.0-bcmath \
                php7.0-curl \
                php7.0-dev \
                php7.0-fpm \
                php7.0-gd \
                php7.0-intl \
                php7.0-mysql \
                php7.0-mcrypt \
                php7.0-mbstring \
                php7.0-recode \
                php7.0-redis \
                php7.0-pspell \
                php7.0-sqlite3 \
                php7.0-sybase \
                php7.0-tidy \
                php7.0-soap \
                php7.0-xml \
                php7.0-xmlrpc \
                php7.0-xsl \
                php7.0-zip \
                php7.0-xdebug

# Disable xdebug by default. On-demand only
RUN phpdismod xdebug

# Add composer
RUN \
    wget -q https://getcomposer.org/composer.phar \
    && chmod +x composer.phar \
    && mv composer.phar /usr/local/bin/composer

# Add Scrutinizer Ocular
RUN \
    wget -q https://scrutinizer-ci.com/ocular.phar \
    && chmod +x ocular.phar \
    && mv ocular.phar /usr/local/bin/ocular


CMD /bin/bash -i
