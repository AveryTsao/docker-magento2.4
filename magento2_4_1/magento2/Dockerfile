FROM php:7.4-apache

LABEL maintainer="averytsaowork@gmail.com"
LABEL php_version="7.4"
LABEL magento_version="2.4.1"
LABEL composer_version="1.10.22"
LABEL description="Magento 2.4.1 with PHP 7.4"

ENV MAGENTO_VERSION 2.4.1
ENV COMPOSER_VERSION 1.10.22
ENV INSTALL_DIR /var/www/html
ENV COMPOSER_HOME /var/www/.composer/

# start up Apache mod_rewrite
RUN a2enmod rewrite

# install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --version=$COMPOSER_VERSION \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

COPY ./auth.json $COMPOSER_HOME

# Install PHP extensions 
RUN set -xe \
    && apt-get update \
    && docker-php-ext-install pdo pdo_mysql \
    && apt-get install -y unzip libzip-dev libgd-dev libicu-dev libfreetype6-dev libxslt-dev libxml2-dev libgcrypt-dev libmcrypt-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install xsl \
    && docker-php-ext-install soap \
    && docker-php-ext-install sockets \
    && docker-php-ext-install bcmath 

RUN yes '' | pecl install mcrypt-1.0.4 \ 
    && echo 'extension=mcrypt.so' > /usr/local/etc/php/conf.d/mcrypt.ini 

RUN chsh -s /bin/bash www-data

RUN chown -R www-data:www-data /var/www

RUN echo "memory_limit=2048M" > /usr/local/etc/php/conf.d/memory-limit.ini

RUN su www-data -c "composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition $INSTALL_DIR $MAGENTO_VERSION"

RUN cd $INSTALL_DIR \
    && find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} \; \
    && find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} \; \
    && chown -R :www-data .  \
    && chmod u+x bin/magento
	
RUN apt-get install --no-install-recommends --assume-yes --quiet ca-certificates curl git \
    && yes '' | apt-get install golang

ENV GOPATH $INSTALL_DIR

RUN go get github.com/mailhog/mhsendmail \
    && mv $GOPATH/bin/mhsendmail /usr/local/bin/mhsendmail \
    && echo 'sendmail_path = mhsendmail --smtp-addr mailhog:1025' > /usr/local/etc/php/php.ini

COPY ./install-magento /usr/local/bin/install-magento
RUN chmod +x /usr/local/bin/install-magento

COPY ./install-sampledata /usr/local/bin/install-sampledata
RUN chmod +x /usr/local/bin/install-sampledata

COPY ./install-upgrade /usr/local/bin/install-upgrade
RUN chmod +x /usr/local/bin/install-upgrade

COPY ./index/phpInfo.php $INSTALL_DIR

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR $INSTALL_DIR
