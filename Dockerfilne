FROM debian:12

RUN apt-get update && \
    apt-get install -y ca-certificates \
    apt-transport-https \
    lsb-release \
    build-essential \
    libxml2-dev \
    libssl-dev \
    libcurl4-openssl-dev \
    apache2 \
    apache2-dev \
    pkg-config \
    python3-launchpadlib \
    software-properties-common \
    zlib1g-dev \
    build-essential \
    libpng-dev \
    libreadline-dev \
    libmariadb-dev-compat \
    libmariadb-dev \
    wget && \
    update-ca-certificates

RUN wget https://www.php.net/distributions/php-5.3.29.tar.gz --no-check-certificate
RUN tar -xzvf php-5.3.29.tar.gz
WORKDIR /php-5.3.29
RUN ./configure --disable-cgi --with-apxs2 --with-mysql --with-pdo-mysql && \
    make && \
    make install

RUN a2dismod mpm_event
RUN a2dismod mpm_worker
RUN a2enmod mpm_prefork

RUN echo "short_open_tag = On" > /usr/local/lib/php.ini
COPY docker-php.conf /etc/apache2/conf-available/
RUN a2enconf docker-php

RUN rm -rf  /php-5.3.29.tar.gz

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

WORKDIR /var/www/html
CMD ["apachectl", "-D", "FOREGROUND"]
