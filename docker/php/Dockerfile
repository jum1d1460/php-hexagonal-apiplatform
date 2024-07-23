FROM php:8.3-fpm-alpine

ARG USER_ID=1000
ARG GROUP_ID=1000

ENV USER_ID=${USER_ID:-1000}
ENV GROUP_ID=${GROUP_ID:-1000}

# Install dependencies
RUN apk update && apk add --no-cache \
    postgresql-dev \
    oniguruma-dev \
    libpng-dev \
    libzip-dev \
    zlib-dev \
    icu-dev \
    libjpeg-turbo-dev \
    libxpm-dev \
    libwebp-dev \
    freetype-dev

# Install PHP extensions
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions \
    bcmath \
    gd \
    intl \
    pdo_pgsql \
    zip \
    @composer

# Crear un nuevo grupo y usuario www-data con los IDs proporcionados
RUN deluser www-data && \
    addgroup -g ${GROUP_ID} www-data && \
    adduser -D -u ${USER_ID} -G www-data www-data

# Asignar permisos a los directorios
RUN chown -R www-data:www-data /var/www/html

# Establecer el directorio de trabajo
WORKDIR /var/www/html

# Copiar configuración PHP personalizada
COPY docker/php/php.ini /usr/local/etc/php/conf.d/

# Usuario y grupo configurado para el contenedor
USER www-data

CMD ["php-fpm"]
