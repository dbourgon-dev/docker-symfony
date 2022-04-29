FROM php:8.1-fpm

ARG UID
EXPOSE ${UID}

RUN adduser --disabled-password -u ${UID} --gecos "" symfonyuser
RUN mkdir /home/symfonyuser/.ssh
RUN chown -R symfonyuser:symfonyuser /home/symfonyuser
RUN chown -R symfonyuser:symfonyuser /var/www
RUN echo "StrictHostKeyChecking no" >> /home/symfonyuser/.ssh/config
RUN echo "export COLUMNS=300" >> /home/symfonyuser/.bashrc
RUN echo "alias sf=/var/www/api/bin/console" >> /home/symfonyuser/.bashrc
#RUN echo "alias symfony=/var/www/bin/console" >> /home/symfonyuser/.bashrc

COPY ./php.ini  /usr/local/etc/php/php.ini

RUN apt update \
    && apt install -y git acl openssl openssh-client wget zip nano libssh-dev \
    && apt install -y libpng-dev zlib1g-dev libzip-dev libxml2-dev libicu-dev \
    && docker-php-ext-install intl pdo pdo_mysql zip gd soap bcmath sockets \
    && pecl install xdebug \
    && docker-php-ext-enable --ini-name 05-opcache.ini opcache xdebug

RUN curl --insecure https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer
RUN composer self-update

RUN wget https://cs.symfony.com/download/php-cs-fixer-v2.phar -O php-cs-fixer
RUN chmod a+x php-cs-fixer

RUN echo 'deb [trusted=yes] https://repo.symfony.com/apt/ /' |  tee /etc/apt/sources.list.d/symfony-cli.list
RUN apt update
RUN apt install symfony-cli
RUN mkdir -p /var/www

USER symfonyuser

RUN chown symfonyuser:symfonyuser /var/www

RUN git config --global user.email "dbourgon.dev@gmail.com"
RUN git config --global user.name "David Bourgon"

WORKDIR /var/www
