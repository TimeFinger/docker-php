FROM php:5.6.37-fpm-alpine3.7
MAINTAINER lujiaming

RUN echo 'https://mirrors.aliyun.com/alpine/v3.7/main' > /etc/apk/repositories \
    && echo 'https://mirrors.aliyun.com/alpine/v3.7/community' >> /etc/apk/repositories

ENV PHPIZE_DEPS \
		autoconf \
		dpkg-dev dpkg \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pcre-dev \
		pkgconf \
		bzip2-dev \
		libxslt-dev \
		re2c
ENV PHPREDIS_VERSION 3.1.0
ENV PHPMEMCACHED_VERSION 2.2.0

RUN apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		&& apk add --no-cache libpng-dev libjpeg-turbo-dev libmcrypt-dev \
		libmemcached-dev cyrus-sasl-dev \
		&& docker-php-ext-install gd mysqli pdo_mysql opcache bcmath mcrypt \
		&& curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
		&& curl -L -o /tmp/memcached.tar.gz https://github.com/php-memcached-dev/php-memcached/archive/$PHPMEMCACHED_VERSION.tar.gz \
		&& tar zxvf /tmp/redis.tar.gz \
		&& tar zxvf /tmp/memcached.tar.gz \
		&& rm -r /tmp/redis.tar.gz \
		&& rm -r /tmp/memcached.tar.gz \
		&& mkdir -p /usr/src/php/ext \
		&& mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
		&& mv php-memcached-$PHPMEMCACHED_VERSION /usr/src/php/ext/memcached \
		&& docker-php-ext-install redis \
		&& docker-php-ext-install memcached \
		&& docker-php-ext-enable redis \
		&& docker-php-ext-enable memcached \
		&& docker-php-source delete \
		&& apk del .build-deps

COPY etc/php5/php-fpm.d/* /usr/local/etc/php-fpm.d/

EXPOSE 9000

CMD ["php-fpm"]
