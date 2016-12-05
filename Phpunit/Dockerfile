# Very simple PHPUnit docker file
#

FROM php:7-apache

RUN set -xe; \
    groupadd -g 1032 alpine && \
    useradd -m -u 1031 -r -g alpine alpine


RUN printf "deb http://packages.dotdeb.org jessie all\ndeb-src http://packages.dotdeb.org jessie all\n" \
        >> /etc/apt/sources.list.d/extras.list

# Disable interactivity
ARG DEBIAN_FRONTEND=noninteractive

# Add the PHP7 repo
RUN curl -s https://www.dotdeb.org/dotdeb.gpg | apt-key add -

RUN apt-get update -y

RUN apt-get install -y \
    git wget zip mysql-client \
    libjpeg62-turbo-dev \
    libmcrypt-dev libpng12-dev \
    libicu-dev libxml2-dev \
    php7.0-bcmath \
    php7.0-xdebug \
    php7.0-cli \
    php7.0-apcu \
    php7.0-igbinary \
    php7.0-mongodb \
    php7.0-redis \
    php7.0-xmlrpc

RUN docker-php-ext-install -j5 \
        gd \
        mcrypt \
        intl \
        pdo_mysql \
        pcntl \
        soap \
        zip;

# Add composer
RUN \
    wget -q http://getcomposer.org/download/1.2.2/composer.phar \
    && chmod +x composer.phar \
    && mv composer.phar /usr/local/bin/composer


# Install Additional extensions
RUN ext_path= \
    && php_ext_dir=$(php-config --extension-dir) && \
    for ext in \
        bcmath \
        igbinary \
        mongodb \
        redis \
        xdebug \
        xmlrpc; \
    do ext_path=$(find  / -name "${ext}"*.so* | head -n 1); \
        if [ -n "${ext_path+set}" ]; then \
            ln -svf "${ext_path}" ${php_ext_dir}/${ext}.so; \
        fi; \
    done

# Enable the extensions
RUN php_ini_path=$(realpath $(php --ini | grep -e ".*\.ini files in" | cut -d':' -f2)) && \
    for ext in \
        bcmath \
        igbinary \
        redis; \
    do printf  "extension=${ext}.so\n" >> "${php_ini_path}/20-${ext}.ini"; done


USER alpine
