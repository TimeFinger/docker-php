FROM php:7.2.10-fpm-alpine3.8
MAINTAINER lujiaming

RUN echo 'https://mirrors.aliyun.com/alpine/v3.8/main' > /etc/apk/repositories \
    && echo 'https://mirrors.aliyun.com/alpine/v3.8/community' >> /etc/apk/repositories

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
ENV PHPREDIS_VERSION 4.1.1
ENV PHPMEMCACHED_VERSION 3.0.4
ENV PHPSWOOLE_VERSION 4.2.1
ENV PHPMONGODB_VERSION 1.5.3
ENV PHPRDKAFKA_VERSION 3.1.2
ENV MONGO_C_DRIVER_VERSION 6afb83ef8dd8a1627bdc879e74bfa88bdd120a4b
ENV PHPREDIS_URL https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz
ENV PHPMEMCACHED_URL https://github.com/php-memcached-dev/php-memcached/archive/v$PHPMEMCACHED_VERSION.tar.gz
ENV PHPSWOOLE_URL https://github.com/swoole/swoole-src/archive/v$PHPSWOOLE_VERSION.tar.gz
ENV PHPMONGODB_URL https://github.com/mongodb/mongo-php-driver/archive/$PHPMONGODB_VERSION.tar.gz
ENV MONGO_C_DRIVER_URL https://github.com/mongodb/mongo-c-driver/archive/$MONGO_C_DRIVER_VERSION.tar.gz
ENV PHPRDKAFKA_URL https://pecl.php.net/get/rdkafka-$PHPRDKAFKA_VERSION.tgz

RUN apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
		&& apk add --no-cache libpng-dev libjpeg-turbo-dev libmemcached-dev cyrus-sasl-dev librdkafka-dev \
		&& docker-php-ext-install gd mysqli pdo_mysql opcache bcmath \
		&& curl -L -o /tmp/rdkafka.tar.gz $PHPRDKAFKA_URL \
		&& curl -L -o /tmp/redis.tar.gz $PHPREDIS_URL \
		&& curl -L -o /tmp/memcached.tar.gz $PHPMEMCACHED_URL \
		&& curl -L -o /tmp/swoole.tar.gz $PHPSWOOLE_URL \
		&& curl -L -o /tmp/mongodb.tar.gz $PHPMONGODB_URL \
		&& curl -L -o /tmp/mongoc.tar.gz $MONGO_C_DRIVER_URL \
		&& tar xf /tmp/rdkafka.tar.gz \
		&& tar xf /tmp/redis.tar.gz \
		&& tar xf /tmp/memcached.tar.gz \
		&& tar xf /tmp/swoole.tar.gz \
		&& tar xf /tmp/mongodb.tar.gz \
		&& tar xf /tmp/mongoc.tar.gz \
		&& rm -r /tmp/rdkafka.tar.gz \
		&& rm -r /tmp/redis.tar.gz \
		&& rm -r /tmp/memcached.tar.gz \
		&& rm -r /tmp/swoole.tar.gz \
		&& rm -r /tmp/mongodb.tar.gz \
		&& mkdir -p /usr/src/php/ext \
		&& mv rdkafka-$PHPRDKAFKA_VERSION /usr/src/php/ext/rdkafka \
		&& mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
		&& mv php-memcached-$PHPMEMCACHED_VERSION /usr/src/php/ext/memcached \
		&& mv swoole-src-$PHPSWOOLE_VERSION /usr/src/php/ext/swoole \
		&& mv mongo-php-driver-$PHPMONGODB_VERSION /usr/src/php/ext/mongodb \
		&& rm -rf /usr/src/php/ext/mongodb/src/libmongoc \
		# 修复找不到 /bson-config.h.in 问题
		&& ln -s /var/www/html/mongo-c-driver-$MONGO_C_DRIVER_VERSION /usr/src/php/ext/mongodb/src/libmongoc \
		&& docker-php-ext-install rdkafka \
		&& docker-php-ext-install redis \
		&& docker-php-ext-install memcached \
		&& docker-php-ext-install swoole \
		&& docker-php-ext-install mongodb \
		&& docker-php-ext-enable rdkafka \
		&& docker-php-ext-enable redis \
		&& docker-php-ext-enable memcached \
		&& docker-php-ext-enable swoole \
		&& docker-php-ext-enable mongodb \
		&& docker-php-source delete \
		&& apk del .build-deps

COPY etc/php7/php-fpm.d/* /usr/local/etc/php-fpm.d/

EXPOSE 9000

CMD ["php-fpm"]
