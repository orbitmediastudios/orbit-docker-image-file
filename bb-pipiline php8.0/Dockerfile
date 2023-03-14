FROM php:8.0.28-buster

ENV DEBIAN_FRONTEND noninteractive
ENV LC_ALL en_US.UTF-8
ENV LANGUAGE en_US:en

RUN \
 apt-get update &&\
 apt-get -y --no-install-recommends install curl locales apt-utils &&\
 echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen &&\
 locale-gen en_US.UTF-8 &&\
 /usr/sbin/update-locale LANG=en_US.UTF-8 &&\
 echo "mysql-server mysql-server/root_password password root" | debconf-set-selections &&\
 echo "mysql-server mysql-server/root_password_again password root" | debconf-set-selections &&\
 apt-get -y --no-install-recommends install ca-certificates gnupg git subversion imagemagick openssh-client lsb-release wget software-properties-common gettext zip unzip default-mysql-server default-mysql-client apt-transport-https ruby python python3 perl memcached geoip-database make ffmpeg rhash &&\
 curl -sSL https://deb.nodesource.com/setup_18.x | bash - &&\
 apt-get -y --no-install-recommends install nodejs &&\
 #apt-get -y --no-install-recommends install php8.0-apcu php8.0-bcmath php8.0-cli php8.0-curl php8.0-gd php8.0-geoip php8.0-gettext php8.0-imagick php8.0-intl php8.0-json php8.0-mbstring php8.0-mysql php8.0-sqlite3 php8.0-xdebug php8.0-xml php8.0-xmlrpc php8.0-zip php8.0-memcached &&\
 apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /var/log/*

#RUN \
# sed -ri -e "s/^variables_order.*/variables_order=\"EGPCS\"/g" /etc/php/8.0/cli/php.ini &&\
# echo "\nmemory_limit=-1" >> /etc/php/8.0/cli/php.ini &&\
# echo "xdebug.max_nesting_level=250" >> /etc/php/8.0/mods-available/xdebug.ini

RUN \
 curl -sSL https://getcomposer.org/installer | php -- --filename=composer --install-dir=/usr/bin &&\
 curl -sSL https://phar.phpunit.de/phpunit.phar -o /usr/bin/phpunit  && chmod +x /usr/bin/phpunit &&\
 curl -sSL https://codeception.com/codecept.phar -o /usr/bin/codecept && chmod +x /usr/bin/codecept &&\
 curl -sSL https://github.com/infection/infection/releases/download/0.17.7/infection.phar -o /usr/bin/infection && chmod +x /usr/bin/infection &&\
 npm install --no-color --production --global gulp-cli webpack mocha grunt yarn n &&\
 rm -rf /root/.npm /tmp/* /var/tmp/* /var/lib/apt/lists/* /var/log/*
