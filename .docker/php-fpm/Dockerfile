FROM php:8.2-fpm

WORKDIR "/application"

# Get frequently used tools
RUN apt-get update && apt-get install -y \
    build-essential \
    libicu-dev \
    libzip-dev \
    libpng-dev \
    libpq-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libonig-dev \
    locales \
    zip \
    unzip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    git \
    curl \
    wget \
    zsh

RUN docker-php-ext-configure zip

RUN docker-php-ext-install \
        bcmath \
        mbstring \
        pcntl \
        intl \
        zip \
        opcache

RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pgsql pdo_pgsql

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# php-cs-fixer tool
RUN wget https://cs.symfony.com/download/php-cs-fixer-v2.phar -O /usr/local/bin/php-cs-fixer
RUN chmod +x /usr/local/bin/php-cs-fixer

# Configure non-root user.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data

RUN chown -R www-data:www-data /application

# Copy and run composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

EXPOSE 9000

CMD ["php-fpm"]
