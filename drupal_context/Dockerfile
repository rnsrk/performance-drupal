FROM drupal:9.4.7-fpm-alpine

MAINTAINER Robert Nasarek "r.nasarek@gnm.de"

# Install build environment
RUN apk add build-base autoconf vim

# Install extensions
RUN pecl install apcu uploadprogress

# Enable extensions
RUN docker-php-ext-enable apcu uploadprogress

# Remove build packages
RUN apk del build-base autoconf

# Custom PHP settings

## PHP
RUN { \
    echo 'max_execution_time = 60'; \
    echo 'max_input_time = 30'; \
    echo 'max_input_nesting_level = 640'; \
    echo 'max_input_vars = 10000'; \
    echo 'memory_limit = 512M'; \
    echo 'upload_max_filesize = 20M'; \
    echo 'max_file_uploads = 50'; \
    echo 'post_max_size = 20M'; \
  } > /usr/local/etc/php/conf.d/99-custom-settings.ini;