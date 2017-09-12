ARG PHP_VERSION=fpm-alpine

FROM php:${PHP_VERSION}

ARG DEV=0

# install the PHP extensions we need
RUN set -ex; \
	if [ $DEV -eq "1" ]; then \
		apk upgrade --update && apk add \
			autoconf \
			make \
			g++ \
			freetype-dev \
			libjpeg-turbo-dev \
			libpng-dev \
			zlib-dev \
			icu-dev; \
		yes | pecl install xdebug && \
		echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug-enable.ini; \
	else \
		apk upgrade --update && apk add \
			freetype-dev \
			libjpeg-turbo-dev \
			libpng-dev \
			zlib-dev \
			icu-dev; \
	fi; \
	rm -rf /var/lib/apt/lists/*; \
	docker-php-ext-configure intl; \ 
	docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
	docker-php-ext-install intl gd mysqli opcache mbstring

COPY index.php /var/www/html/index.php
COPY wp-config.php /var/www/html/wp-config.php

RUN mkdir -p /var/www/vendor /var/www/html/wordpress /var/www/html/wp-content/themes /var/www/html/wp-content/plugins /var/www/html/wp-content/uploads /var/www/html/wp-content/upgrade /var/www/html/wp-content/languages /var/www/html/wp-content/mu-plugins /var/www/html/wp-content/blogs.dir; \
	chown -R 82:82 /var/www/vendor /var/www/html

COPY config.ini /usr/local/etc/php/conf.d/config.ini

VOLUME ["/var/www/vendor", "/var/www/html/wordpress", "/var/www/html/wp-content/themes", "/var/www/html/wp-content/plugins", "/var/www/html/wp-content/uploads", "/var/www/html/wp-content/upgrade", "/var/www/html/wp-content/languages", "/var/www/html/wp-content/mu-plugins", "/var/www/html/wp-content/blogs.dir"]

EXPOSE 9000

CMD ["php-fpm"]