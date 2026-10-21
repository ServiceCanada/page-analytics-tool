FROM php:8.4.27-fpm-alpine

# PHP 8.4.27 is the first 8.4 release patched for PHP2Shell
# (CVE-2026-109618/109619). Keep this tag on a current security release.
RUN apk add --no-cache fcgi \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS openssl-dev \
    && curl -fsSL -o /tmp/mongodb-2.5.5.tgz \
        https://github.com/mongodb/mongo-php-driver/releases/download/2.5.5/mongodb-2.5.5.tgz \
    && echo "0eaf84ff709f73183d80597f31ff02937f6aefde01eb895a0aa19334a290cec0  /tmp/mongodb-2.5.5.tgz" | sha256sum -c - \
    && pecl install /tmp/mongodb-2.5.5.tgz \
    && docker-php-ext-enable opcache \
    && apk del .build-deps \
    && rm -rf /tmp/pear /tmp/mongodb-2.5.5.tgz

# Used by the Kubernetes liveness and readiness probes.
RUN curl -fsSL -o /usr/local/bin/php-fpm-healthcheck \
        https://raw.githubusercontent.com/renatomefi/php-fpm-healthcheck/v0.6.0/php-fpm-healthcheck \
    && echo "53bc616c4a30f029b98bff48fdeb0c4da252cb11e4f86656a8222a67dc4e5009  /usr/local/bin/php-fpm-healthcheck" | sha256sum -c - \
    && chmod +x /usr/local/bin/php-fpm-healthcheck

COPY docker/performance_php/php.ini /usr/local/etc/php/php.ini
COPY docker/performance_php/www.conf /usr/local/etc/php-fpm.d
COPY assets/ /var/www/html/assets/
COPY js/ /var/www/html/js/
COPY php/ /var/www/html/php/
COPY docker/performance_php/secrets/php/config.php /var/www/html/php/config.php
COPY docker/performance_php/secrets/php/config-at.php /var/www/html/php/config-at.php
COPY docker/performance_php/secrets/php/service-account-credentials.json /var/www/html/php/service-account-credentials.json
COPY docker/performance_php/secrets/keys /var/www/html/keys
#COPY docker/performance_php/mongo_connect.php /var/www/html/php/mongo_connect.php
#COPY docker/performance_php/helloworld.php /var/www/html/php/helloworld.php
COPY index.html /var/www/html/index.html

# Reset Cache
RUN php -version
RUN php -r 'opcache_reset();'
