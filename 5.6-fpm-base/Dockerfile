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
		#fixed swoole make error: linux/aio_abi.h
		linux-headers \
		re2c
ENV PHPREDIS_VERSION 3.1.0
ENV PHPMEMCACHED_VERSION 2.2.0
ENV PHPSWOOLE_VERSION 2.0.11
ENV PHPMONGO_VERSION 1.6.16
ENV PHPREDIS_URL https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz
ENV PHPMEMCACHED_URL https://github.com/php-memcached-dev/php-memcached/archive/$PHPMEMCACHED_VERSION.tar.gz
ENV PHPSWOOLE_URL https://pecl.php.net/get/swoole-$PHPSWOOLE_VERSION.tgz
ENV PHPMONGO_URL https://github.com/mongodb/mongo-php-driver-legacy/archive/$PHPMONGO_VERSION.tar.gz

RUN apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		&& apk add --no-cache libpng-dev libjpeg-turbo-dev libmcrypt-dev \
		libmemcached-dev cyrus-sasl-dev openssl-dev \
		&& docker-php-ext-install gd mysqli pdo_mysql opcache bcmath mcrypt \
		&& curl -L -o /tmp/redis.tar.gz $PHPREDIS_URL \
		&& curl -L -o /tmp/memcached.tar.gz $PHPMEMCACHED_URL \
		&& curl -L -o /tmp/swoole.tar.gz $PHPSWOOLE_URL \
		&& curl -L -o /tmp/mongo.tar.gz $PHPMONGO_URL \
		&& tar xf /tmp/redis.tar.gz \
		&& tar xf /tmp/memcached.tar.gz \
		&& tar xf /tmp/swoole.tar.gz \
		&& tar xf /tmp/mongo.tar.gz \
		&& rm -r /tmp/redis.tar.gz \
		&& rm -r /tmp/memcached.tar.gz \
		&& rm -r /tmp/swoole.tar.gz \
		&& rm -r /tmp/mongo.tar.gz \
		&& mkdir -p /usr/src/php/ext \
		&& mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
		&& mv php-memcached-$PHPMEMCACHED_VERSION /usr/src/php/ext/memcached \
		&& mv swoole-$PHPSWOOLE_VERSION /usr/src/php/ext/swoole \
		&& mv mongo-php-driver-legacy-$PHPMONGO_VERSION /usr/src/php/ext/mongo \
		&& docker-php-ext-install redis \
		&& docker-php-ext-install memcached \
		&& docker-php-ext-install swoole \
		&& docker-php-ext-install mongo \
		&& docker-php-ext-enable redis \
		&& docker-php-ext-enable memcached \
		&& docker-php-ext-enable swoole \
		&& docker-php-ext-enable mongo \
		&& docker-php-source delete \
		&& apk del .build-deps

COPY etc/php5/php-fpm.d/* /usr/local/etc/php-fpm.d/

EXPOSE 9000

CMD ["php-fpm"]
