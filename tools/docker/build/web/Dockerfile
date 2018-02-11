FROM	php:7.2.0-apache

RUN a2enmod rewrite &&\
	a2enmod headers &&\
	a2enmod ssl

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

RUN apt-get update && apt-get -y install mc vim redis-tools build-essential tcl8.5 gnupg git curl mysql-client libcurl4-openssl-dev zlib1g-dev fontconfig openssh-client --no-install-recommends && rm -r /var/lib/apt/lists/*
RUN docker-php-ext-install pdo pdo_mysql curl mbstring zip mysqli
#RUN pecl install xdebug
#RUN docker-php-ext-enable xdebug

#RUN echo "date.timezone = 'UTC'" >> /usr/local/etc/php/php.ini
#RUN echo "memory_limit = 256M" >> /usr/local/etc/php/php.ini
#RUN echo "xdebug.remote_enable = 1" >> /usr/local/etc/php/php.ini
#RUN echo "xdebug.idekey = 'netbeans-xdebug'" >> /usr/local/etc/php/php.ini
#RUN echo "xdebug.remote_connect_back = 'on'" >> /usr/local/etc/php/php.ini

RUN git config --global url."https://".insteadOf git://
RUN git config --global core.fileMode false

RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

# Node.js
RUN curl -sL https://deb.nodesource.com/setup_9.x | bash -
RUN apt-get update && apt-get install -y nodejs

RUN npm -g config set user root
RUN npm install -g @angular/cli

#RUN git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis \
#	&& docker-php-ext-install redis

# PHPUnit
RUN curl -L -O https://phar.phpunit.de/phpunit-6.5.phar
RUN chmod +x phpunit-6.5.phar
RUN mv phpunit-6.5.phar /usr/local/bin/phpunit

# Apache
ADD ./hosts/* /etc/apache2/sites-available/

RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/wallet.key \
    -out /etc/ssl/certs/wallet.pem

RUN cd /etc/apache2/sites-available/ && a2dissite * && a2ensite wallet-ssl.conf
RUN usermod -s /bin/bash www-data
RUN mkdir /var/www/php_tmp
RUN chown -R www-data: /var/www/php_tmp

WORKDIR /var/www/sources

EXPOSE 80 8080 443 3000 9000 4200
