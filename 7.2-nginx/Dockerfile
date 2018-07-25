FROM alpine:3.8
MAINTAINER lujiaming

RUN echo 'https://mirrors.aliyun.com/alpine/v3.8/main' > /etc/apk/repositories \
    && echo 'https://mirrors.aliyun.com/alpine/v3.8/community' >> /etc/apk/repositories

RUN apk add --no-cache supervisor nginx php7 php7-common php7-fpm php7-apcu php7-bcmath php7-curl php7-ctype php7-gd php7-gettext php7-iconv php7-json php7-mcrypt php7-mbstring php7-memcached php7-mysqlnd php7-mysqli php7-pdo php7-pdo_mysql php7-redis php7-session php7-xml php7-zip

RUN mkdir /etc/supervisor.d && mkdir /run/nginx && mkdir /www
COPY etc/php7/php-fpm.d/* /etc/php7/php-fpm.d/
COPY etc/supervisor.d/* /etc/supervisor.d/
COPY etc/nginx/conf.d/* /etc/nginx/conf.d/
COPY www/* /www/

HEALTHCHECK --interval=5s --timeout=3s \
  CMD netstat -antpl | grep nginx && ps aux | grep php-fpm | grep -v grep || exit 1

EXPOSE 80

CMD ["supervisord", "-n", "-c", "/etc/supervisord.conf"]
