FROM php:5.6

# Install PHP extension dependencies
RUN buildDeps=" \
        libpng-dev \
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

# Install MySQL
RUN apt-get update \
    && apt-get install -qy mysql-client \
    && apt-get purge -y --auto-remove

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir=bin

# Install Git
RUN apt-get update -qy \
    && apt-get install -qy git-core \
    && apt-get purge -y --auto-remove

# Install magerun
RUN curl -o magerun https://files.magerun.net/n98-magerun.phar \
    && chmod +x ./magerun \
    && ./magerun selfupdate \
    && cp ./magerun /usr/local/bin/ \
    && rm ./magerun

# Create app directories
RUN mkdir /var/www \
    && mkdir /var/www/html \
    && mkdir /var/www/html/web \
