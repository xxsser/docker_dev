FROM php:fpm
ADD php.ini    /usr/local/etc/php/php.ini

# ADD php-fpm.conf    /usr/local/etc/php-fpm.conf

RUN apt-get update && apt-get install -y \
        libmcrypt-dev \
        libicu-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
        libgmp-dev \
    && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include \
    && docker-php-ext-configure gmp \
    && docker-php-ext-install gmp \
    && echo extension=gmp.so > /usr/local/etc/php/conf.d/gmp.ini
    && docker-php-ext-install -j$(nproc) mcrypt \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_php \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install zip \
    && docker-php-ext-install bcmath \
    && pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_mode=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_mode=\"req\"" >> /usr/local/etc/php/conf.d/xdebug.ini
    && echo "xdebug.remote_handler=\"dbgp\"" >> /usr/local/etc/php/conf.d/xdebug.ini
    && echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/xdebug.ini

ADD  redis-php7/*    /home/redis-php7/
RUN  cd /home/redis-php7 && phpize && ./configure && make && make install \
	 && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

ADD composer.phar /usr/local/bin/composer
RUN chmod 755 /usr/local/bin/composer
WORKDIR /opt
RUN usermod -u 1000 www-data
VOLUME ["/opt"]