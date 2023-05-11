FROM drupal:10.0.9-php8.2-fpm-alpine

MAINTAINER Robert Nasarek "r.nasarek@gnm.de"

# Install all the stuff we need

# Install packages 
RUN	apk update && \
	apk	upgrade -U

RUN	apk add \
		autoconf \
		automake \
		build-base \
		coreutils \
		freetype-dev \
		git \
		imagemagick \
		jpeg-dev \
		libpng-dev \
		libpq-dev \
		libtool \
		libwebp-dev \
		libwebp-tools \
		libxml2-dev \
		libzip-dev \
		linux-headers \
		postgresql-dev \
		tiff-dev \
		unzip \
		vim \
		wget;

# Add php extensions
RUN	docker-php-ext-configure gd \
		--with-freetype \
		--with-jpeg=/usr \
		--with-webp;

RUN	docker-php-ext-install -j "$(nproc)" \
		dom \
		gd \
		opcache \
		pdo_mysql \
		pdo_pgsql \
		zip;

# Runtime Dependencies
RUN runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --no-network --virtual .drupal-phpexts-rundeps $runDeps;

# Upload progress
RUN	set -eux; \
	git clone https://github.com/php/pecl-php-uploadprogress/ /usr/src/php/ext/uploadprogress/; \
	docker-php-ext-configure uploadprogress; \
	docker-php-ext-install uploadprogress; \
	rm -rf /usr/src/php/ext/uploadprogress;

# Install apcu
RUN set -eux; \
	pecl install apcu; \
	pecl install xdebug \
    docker-php-ext-enable xdebug; 


# Add php configs
RUN { \
		echo 'extension=apcu.so'; \
		echo "apc.enable_cli=1"; \
		echo "apc.enable=1"; \
	} >> /usr/local/etc/php/php.ini;

# Install iipsrv
RUN set -eux; \
	git clone https://github.com/ruven/iipsrv.git; \
	cd iipsrv; \
	./autogen.sh; \
	./configure; \
	make; \
	mkdir /fcgi-bin; \
	cp src/iipsrv.fcgi /fcgi-bin/iipsrv.fcgi

# Add IIPServer config
COPY iipsrv.conf /etc/apache2/mods-available/iipsrv.conf 

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
COPY 99-custom-opcache-settings.ini /usr/local/etc/php/conf.d/99-custom-opcache-settings.ini;

# Tweek PHP
COPY 99-custom-php-settings.ini /usr/local/etc/php/conf.d/99-custom-php-settings.ini;

# Install composer
COPY --from=composer:2 /usr/bin/composer /usr/local/bin/

# Add private files directory
RUN mkdir /var/www/private_files && chown -R www-data /var/www/private_files

# Add Drupal directory
RUN mkdir -p /opt/drupal

# Change workdir
WORKDIR /opt/drupal

# Copy entrypoint to image
COPY entrypoint.sh /usr/local/bin
RUN chmod +x /usr/local/bin/entrypoint.sh

# Copy Drupal settings from context to image 
COPY settings.php /settings.php

# (Re)link html dir
RUN ln -sf /opt/drupal/web /var/www/html

ENV PATH=${PATH}:/opt/drupal/vendor/bin

# Define entrypoint
ENTRYPOINT entrypoint.sh

# vim:set ft=dockerfile:
