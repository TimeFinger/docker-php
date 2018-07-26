FROM alpine:3.8
MAINTAINER lujiaming

RUN echo 'https://mirrors.aliyun.com/alpine/v3.8/main' > /etc/apk/repositories \
    && echo 'https://mirrors.aliyun.com/alpine/v3.8/community' >> /etc/apk/repositories

RUN apk add --no-cache php7 php7-common php7-fpm php7-apcu php7-bcmath php7-curl php7-ctype php7-gd php7-gettext php7-iconv php7-json php7-mcrypt php7-mbstring php7-memcached php7-mysqlnd php7-mysqli php7-pdo php7-pdo_mysql php7-redis php7-session php7-xml php7-zip

COPY etc/php7/php-fpm.d/* /etc/php7/php-fpm.d/

EXPOSE 9000

CMD ["php-fpm7", "-F"]
