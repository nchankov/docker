FROM php:7.4-apache

LABEL author="nik@chankov.net"

#configuration to come

ENV APACHE_DOCUMENT_ROOT /var/www

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN echo 'RewriteMap upper int:toupper' >> /etc/apache2/sites-available/000-default.conf

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

#apache enable rewrite
RUN a2enmod rewrite

#apache enable headers
RUN a2enmod headers

#Add html to pdf
RUN apt-get update && apt-get install -y --no-install-recommends && apt-get -y install wkhtmltopdf

#install and configure gd
RUN apt-get update && apt-get install -y --no-install-recommends \
	build-essential \
	zlib1g \
	libpng-dev \
	&& docker-php-ext-configure gd --enable-gd && docker-php-ext-install -j$(nproc) gd

#install and configure pspell
RUN apt-get update && apt-get install -y --no-install-recommends \
	libpspell-dev && \
	rm -r /var/lib/apt/lists/* && \
	docker-php-ext-install -j$(nproc) pspell

#install and configure zip
RUN apt-get update && apt-get install -y libzip-dev \
	&& docker-php-ext-install zip

#install and configure intl
RUN apt-get -y update \
	&& apt-get install -y libicu-dev \ 
	&& docker-php-ext-configure intl \ 
	&& docker-php-ext-install intl

#install and configure mysqli
RUN docker-php-ext-install mysqli

WORKDIR /var/www
