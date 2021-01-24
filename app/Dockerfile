FROM php:7.4-fpm-alpine

RUN apk add --update --no-cache make git libzip-dev \
    && docker-php-ext-install zip pdo_mysql

ARG PUID=1000
ARG PGID=1000

ENV PUID ${PUID}
ENV PGID ${PGID}

RUN groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data

WORKDIR /app
COPY ./webroot /app

RUN make /bin/composer
