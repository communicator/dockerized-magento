FROM php:5.6-fpm

# Install PHP extension dependencies
RUN buildDeps=" \
        libpng12-dev \
        libjpeg-dev \
        libmcrypt-dev \
        libxml2-dev \
        freetype* \
    "; \
    set -x \
    && apt-get update \
    && apt-get install -y $buildDeps --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install \
    gd \
    mbstring \
    mysqli \
    mcrypt \
    mysql \
    pdo_mysql \
    soap \
    zip

# Configure GD extension
RUN docker-php-ext-configure gd \
    --with-png-dir=/usr \
    --with-jpeg-dir=/usr \
    --with-freetype-dir

# Install XDebug
RUN apt-get update -qy \
    && apt-get install -qy git-core \
    && cd /tmp/ \
        && git clone https://github.com/derickr/xdebug.git \
        && cd xdebug \
        && git checkout xdebug_2_5 \
        && phpize \
        && ./configure --enable-xdebug \
        && make \
        && mkdir /usr/lib/php5/ && cp modules/xdebug.so /usr/lib/php5/xdebug.so \
        && touch /usr/local/etc/php/ext-xdebug.ini \
        && rm -r /tmp/xdebug \
    && apt-get purge -y git-core \
    && apt-get purge -y --auto-remove

# Copy configuration files
COPY php.ini /usr/local/etc/php/php.ini
COPY php-fpm.conf /usr/local/etc/
COPY ext-xdebug.ini /usr/local/etc/php/conf.d/ext-xdebug.ini

# Make sure the volume mount point is empty
RUN rm -rf /var/www/html/*

# Install magerun
RUN curl -o magerun https://files.magerun.net/n98-magerun.phar \
    && chmod +x ./magerun \
    && cp ./magerun /usr/local/bin/ \
    && rm ./magerun \
    && apt-get update \
    && apt-get install -qy mysql-client \
    && apt-get purge -y --auto-remove
