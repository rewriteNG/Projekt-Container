FROM php:7.4-fpm
WORKDIR "/application"
# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive
# Install selected extensiond and other stuff
# RUN apt-get update \
#     && apt-get -y --no-install-recommends install php7.4-mysql libzip-dev zip unzip \
#     && apt-get clean; rm -rf /var/lip/apt/lists* /temp/* /var/temp/* /usr/share/doc/* \
#     && docker-php-ext-configure zip
# RUN docker-php-ext-install pdo pdo_mysql zip
# Install "curl", "libmemcached-dev", "libpq-dev", "libjpeg-dev",
#         "libpng-dev", "libfreetype6-dev", "libssl-dev", "libmcrypt-dev",
RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
    git  \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libonig-dev; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    # Install the PHP pdo_mysql extention
    docker-php-ext-install pdo_mysql; \
    # Install the PHP pdo_pgsql extention
    docker-php-ext-install pdo_pgsql; \
    # Install the PHP gd library
    docker-php-ext-configure gd \
    --prefix=/usr \
    --with-jpeg \
    --with-freetype; \
    docker-php-ext-install gd; \
    php -r 'var_dump(gd_info());'

RUN set -xe; \
    apt-get update -yqq && \
    pecl channel-update pecl.php.net && \
    apt-get install -yqq \
    apt-utils \
    #
    #--------------------------------------------------------------------------
    # Mandatory Software's Installation
    #--------------------------------------------------------------------------
    #
    # Mandatory Software's such as ("mcrypt", "pdo_mysql", "libssl-dev", ....)
    # are installed on the base image 'laradock/php-fpm' image. If you want
    # to add more Software's or remove existing one, you need to edit the
    # base image (https://github.com/Laradock/php-fpm).
    #
    # next lines are here becase there is no auto build on dockerhub see https://github.com/laradock/laradock/pull/1903#issuecomment-463142846
    libzip-dev zip unzip && \
    docker-php-ext-configure zip; \
    # Install the zip extension
    docker-php-ext-install zip && \
    php -m | grep -q 'zip'


# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# RUN apt-get update && apt-get install -y cron && which cron && \
#     rm -rf /etc/cron.*/*

# COPY laravel-cron /etc/cron.d/
# RUN chmod 0644 /etc/cron.d/laravel-cron

# COPY entrypoint.sh /entrypoint.sh

# ENTRYPOINT ["/entrypoint.sh"]
# CMD ["php-fpm"]

RUN apt-get update && apt-get -y install nano 