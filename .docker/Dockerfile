FROM php:7.3-apache
RUN apt-get update && apt-get install -y libicu-dev git zip unzip && docker-php-ext-install intl && docker-php-ext-install pdo_mysql
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
RUN git clone git://github.com/vvaswani/jade.git . && git checkout master
COPY .docker/config/php.ini "$PHP_INI_DIR/conf.d/app.ini"
COPY . /var/www/html/
WORKDIR /var/www/html
RUN curl https://getcomposer.org/installer > composer-setup.php && php composer-setup.php
RUN php composer.phar install --prefer-dist --no-scripts --no-dev --no-autoloader && rm -rf /root/.composer
RUN php composer.phar dump-autoload --no-scripts --no-dev --optimize
RUN rm -rf composer-setup.php composer.phar
RUN apt-get remove -y libicu-dev git zip unzip && apt-get autoremove -y
RUN curl https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh > /var/www/html/.docker/scripts/wait-for-it.sh && chmod +x /var/www/html/.docker/scripts/wait-for-it.sh
RUN chown -R www-data:www-data /var/www/html && a2enmod rewrite
EXPOSE 80