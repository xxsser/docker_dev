FROM php:fpm
ADD php.ini    /usr/local/etc/php/php.ini

# ADD php-fpm.conf    /usr/local/etc/php-fpm.conf
# set aliyun source
RUN truncate -s 0 /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/debian/ jessie main non-free contrib" > /etc/apt/sources.list \
    && echo "deb http://mirrors.aliyun.com/debian/ jessie-proposed-updates main non-free contrib"  >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/debian/ jessie main non-free contrib"  >> /etc/apt/sources.list \
    && echo "deb-src http://mirrors.aliyun.com/debian/ jessie-proposed-updates main non-free contrib" >> /etc/apt/sources.list
RUN apt-get update && apt-get install -y libmcrypt-dev libicu-dev libfreetype6-dev libjpeg62-turbo-dev libpng12-dev libgmp-dev libmagickwand-dev libmagickcore-dev \
    && ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include \
    && docker-php-ext-configure gmp \
    && docker-php-ext-install gmp \
    && docker-php-ext-install -j$(nproc) mcrypt \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mysqli pdo pdo_mysql mbstring zip bcmath \
    && pecl install imagick \
    && echo "extension=$(find /usr/local/lib/php/extensions/ -name imagick.so)" > /usr/local/etc/php/conf.d/imagick.ini \
    && pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_mode=\"req\"" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_handler=\"dbgp\"" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=192.168.99.1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && curl -L -o /tmp/php_redis.tar.gz https://github.com/phpredis/phpredis/archive/3.0.0.tar.gz \
    && tar xfz /tmp/php_redis.tar.gz \
    && rm -r /tmp/php_redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-3.0.0 /usr/src/php/ext/redis \
    && docker-php-ext-install redis \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer \
    && chmod 755 /usr/local/bin/composer
WORKDIR /data
RUN usermod -u 1000 www-data
VOLUME ["/data"]