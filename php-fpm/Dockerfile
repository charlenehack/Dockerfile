FROM php:7.2-fpm-alpine
LABEL maintainer="liucl <119215719@qq.com>"

ARG PHP_CONFIG="/usr/local/etc"
ARG MCRYPT_VERSION="mcrypt-1.0.1"

COPY etc/php/php.ini-production ${PHP_CONFIG}/php/php.ini
COPY etc/php-fpm.d/www.conf ${PHP_CONFIG}/php-fpm.d/www.conf
ADD ImageMagick.tar.gz https://pecl.php.net/get/${MCRYPT_VERSION}.tgz /tmp/

RUN addgroup -S www && adduser -D -S -H -s /sbin/nologin -G www www && \
	apk add --no-cache build-base autoconf libpng-dev libxml2-dev libxslt-dev gettext-dev libmcrypt-dev librdkafka-dev && \
	cd /tmp && tar xvf ${MCRYPT_VERSION}.tgz &&  \
	docker-php-ext-install gd xmlrpc bcmath gettext mysqli pdo_mysql rcntl shmop soap sockets sysvsem xsl opcache zip /tmp/${MCRYPT_VERSION} && \
	cd /tmp/ImageMagick* && ./configure && make && make install && pecl install redis mongodb imagick swoole rdkafka && \
	apk del build-base && rm -rf /tmp/* && \
	sed -i '/;extension=snmp/a\extension=redis.so\nextension=mongodb.so\nextension=imagick.so\nextension=swoole.so\nextension=rdkafka.so' ${PHP_CONFIG}/php/php.ini

EXPOSE 9000
CMD ["php-fpm"]
ENTRYPOINT ["docker-php-entrypoint"]



