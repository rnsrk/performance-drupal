FROM drupal:9.4.7-fpm-alpine

MAINTAINER Robert Nasarek "r.nasarek@gnm.de"

# Install build environment
RUN apk add build-base autoconf vim git

# Install extensions
RUN pecl install apcu uploadprogress

# Enable extensions
RUN docker-php-ext-enable apcu uploadprogress

# Remove build packages
RUN apk del build-base autoconf