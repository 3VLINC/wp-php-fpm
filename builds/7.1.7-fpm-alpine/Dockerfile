FROM php:7.1.7-fpm-alpine

# install the PHP extensions we need
RUN set -ex; \
	\
	apk upgrade --update && \
	apk add \
		freetype-dev \
		libjpeg-turbo-dev \
		libpng-dev \
		zlib-dev \
		icu-dev \
	; \
	rm -rf /var/lib/apt/lists/*; \
	\
	docker-php-ext-configure intl; \ 
	docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
	docker-php-ext-install intl gd mysqli opcache mbstring

ADD config.ini /usr/local/etc/php/conf.d/config.ini

VOLUME ["/var/www/html"]

EXPOSE 9000

CMD ["php-fpm"]