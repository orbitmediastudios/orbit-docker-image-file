FROM php:8.3-apache
# install the PHP extensions we need
RUN apt-get update && apt-get install -y libpng-dev libjpeg-dev libmcrypt-dev libxml2-dev libxslt1-dev libzip-dev libonig-dev libfreetype6-dev git curl libmcrypt-dev libcurl3-dev libssh2-1-dev libssh2-1 && rm -rf /var/lib/apt/lists/* \
	&& docker-php-ext-configure gd --with-jpeg=/usr/include --with-freetype=/usr/include/ \
	&& docker-php-ext-install gd
    #&& pecl install ssh2-1.2
    #&& docker-php-ext-enable ssh2
RUN docker-php-ext-install gettext
#RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install soap
RUN docker-php-ext-install sockets
RUN docker-php-ext-install curl
#RUN docker-php-ext-install xmlrpc
#RUN docker-php-ext-install tokenizer
RUN docker-php-ext-install exif
RUN docker-php-ext-install xml
RUN docker-php-ext-install zip
RUN docker-php-ext-install mbstring
#RUN docker-php-ext-install ssh2

RUN apt-get update && apt-get install -y libmemcached-dev zlib1g-dev \
    && pecl install memcached \
    && docker-php-ext-enable memcached

RUN pecl install redis-5.3.7 \
    && docker-php-ext-enable redis

RUN echo "date.timezone = America/Chicago" > /usr/local/etc/php/php.ini
RUN echo "log_errors = on" >> /usr/local/etc/php/php.ini
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/php.ini
RUN echo "upload_max_filesize = 100M" >> /usr/local/etc/php/php.ini
RUN echo "post_max_size = 100M" >> /usr/local/etc/php/php.ini
RUN echo "max_execution_time = 180" >> /usr/local/etc/php/php.ini

RUN echo "soap.wsdl_cache_enabled = 0" >> /usr/local/etc/php/php.ini
RUN echo "soap.wsdl_cache = 0" >> /usr/local/etc/php/php.ini
RUN echo "default_socket_timeout = 200" >> /usr/local/etc/php/php.ini


# Make Ubuntu great again! Enable SSL and mod_rewrite by default! Attach key and self sign SSL certifcate!
# It's going to be great, believe me!
COPY apache.key /etc/ssl/private/apache.key
COPY apache.crt /etc/ssl/certs/apache.crt
COPY default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

RUN a2enmod ssl
RUN a2ensite default-ssl
# Really Ubuntu?
RUN a2enmod rewrite

# set CLI tools:
# Composer
RUN curl -o /usr/local/bin/wp https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && chmod +x /usr/local/bin/wp
# WP CLI
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && chmod +x /usr/local/bin/composer
# Drush - install drush locally with composer - belaow doesn't work anymore
#RUN php -r "readfile('http://files.drush.org/drush.phar');" > /usr/local/bin/drush && chmod +x /usr/local/bin/drush

#Drupal Console
RUN php -r "readfile('https://drupalconsole.com/installer');" > /usr/local/bin/drupal && chmod +x /usr/local/bin/drupal

# Code Sniffer
RUN curl -o /usr/local/bin/phpcs https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar && chmod +x /usr/local/bin/phpcs
RUN curl -o /usr/local/bin/phpcbf https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar && chmod +x /usr/local/bin/phpcbf

# Add WPCS
RUN git clone -b main https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards.git /usr/share/wpcs
# Add Drupal CS
RUN git clone --branch 8.3.x http://git.drupal.org/project/coder.git /usr/share/drupal_cs

# Connect Code Sniffer with Wordpress, Drupal CS - add project root directory for custom
RUN phpcs --config-set installed_paths /usr/share/wpcs,/usr/share/drupal_cs/coder_sniffer,/var/www/html

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
        echo 'opcache.memory_consumption=128'; \
        echo 'opcache.interned_strings_buffer=8'; \
        echo 'opcache.max_accelerated_files=4000'; \
        echo 'opcache.revalidate_freq=60'; \
        echo 'opcache.fast_shutdown=1'; \
        echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini