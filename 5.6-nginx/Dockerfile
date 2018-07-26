FROM hewn2011/php:5.6.37-fpm-base
MAINTAINER lujiaming

RUN apk add --no-cache supervisor nginx
RUN mkdir /etc/supervisor.d && mkdir /run/nginx && mkdir /www

COPY etc/php5/php-fpm.d/* /usr/local/etc/php-fpm.d/
COPY etc/supervisor.d/* /etc/supervisor.d/
COPY etc/nginx/conf.d/* /etc/nginx/conf.d/
COPY www/* /www/

HEALTHCHECK --interval=5s --timeout=3s \
  CMD netstat -antpl | grep nginx && ps aux | grep php-fpm | grep -v grep || exit 1

EXPOSE 80

WORKDIR /run

CMD ["supervisord", "-n", "-c", "/etc/supervisord.conf"]
