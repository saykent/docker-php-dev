FROM ubuntu:20.04

LABEL maintainer="Ken H. You"

ARG WWWGROUP

WORKDIR /var/www/html

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=UTC

ENV APACHE_CONFDIR /etc/apache2

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y gnupg gosu curl ca-certificates zip unzip git supervisor sqlite3 libcap2-bin libpng-dev python2 \
    && mkdir -p ~/.gnupg \
    && chmod 600 ~/.gnupg \
    && echo "disable-ipv6" >> ~/.gnupg/dirmngr.conf \
    && apt-key adv --homedir ~/.gnupg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E5267A6C \
    && apt-key adv --homedir ~/.gnupg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C300EE8C \
    && echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu focal main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
    && apt-get update \
    && apt-get install -y php7.4-cli php7.4-dev \
       php7.4-pgsql php7.4-sqlite3 php7.4-gd \
       php7.4-curl php7.4-memcached \
       php7.4-imap php7.4-mysql php7.4-mbstring \
       php7.4-xml php7.4-zip php7.4-bcmath php7.4-soap \
       php7.4-intl php7.4-readline php7.4-pcov \
       php7.4-msgpack php7.4-igbinary php7.4-ldap \
       php7.4-redis \
    && apt-get install -y --no-install-recommends apache2 libapache2-mod-php7.4\
    && apt-get install php7.4-xdebug\
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && curl -sL https://deb.nodesource.com/setup_15.x | bash - \
    && apt-get install -y nodejs \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get install -y yarn \
    && apt-get install -y mysql-client \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN echo "xdebug.mode=debug" >> /etc/php/7.4/apache2/conf.d/20-xdebug.ini
RUN echo "xdebug.client_host=host.docker.internal" >> /etc/php/7.4/apache2/conf.d/20-xdebug.ini

RUN setcap "cap_net_bind_service=+ep" /usr/bin/php7.4

# Apache + PHP requires preforking Apache for best results
RUN a2dismod mpm_event && a2enmod mpm_prefork

RUN groupadd --force -g $WWWGROUP phpdev
RUN useradd -ms /bin/bash --no-user-group -g $WWWGROUP -u 1337 phpdev

COPY 000-default.conf /etc/apache2/sites-available/
COPY apache2-foreground /usr/local/bin/
COPY php.ini /etc/php/7.4/cli/conf.d/99-phpdev.ini

RUN chmod +x /usr/local/bin/apache2-foreground

WORKDIR /var/www/html
EXPOSE 80

CMD ["apache2-foreground"]
