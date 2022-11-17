FROM php:8.0-apache
ARG APP_FOLDER
ARG APP_CRON
ENV APP_FOLDER=$APP_FOLDER
ENV APP_CRON=$APP_CRON

# Set the Drush version.
ENV DRUSH_VERSION 10.6.2

# Set the composer version.
ENV COMPOSER_VERSION 2.3.4

# Enable rewrite headers
RUN a2enmod rewrite headers

# Intall mysqli php extention
RUN docker-php-ext-install mysqli

# Setup Debian
RUN apt-get upgrade && apt-get update && ACCEPT_EULA=Y && apt-get install -y \
        unzip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libzip-dev \
        libgeoip-dev \
        libxml2-dev \
        libxslt-dev \
        libtidy-dev \
        libssl-dev \
        zlib1g-dev \
        libpng-dev \
        libjpeg62-turbo-dev \
        libfreetype6-dev \
        libaio1 \
        apt-file \
        wget \
        vim \
        gnupg \
        gnupg2 \
        zip \
        git \
	      npm \
        nodejs \
        libonig-dev \
        cron

RUN docker-php-ext-install -j$(nproc) calendar iconv bcmath xml mbstring pdo tidy gettext intl pdo pdo_mysql mysqli simplexml tokenizer xml xsl xmlwriter zip opcache exif 

RUN docker-php-ext-configure zip

    
# Install SSL
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ssl-cert-snakeoil.key -out /etc/ssl/certs/ssl-cert-snakeoil.pem -subj "/C=AT/ST=Vienna/L=Vienna/O=Security/OU=Development/CN=example.com"
RUN a2ensite default-ssl
RUN a2enmod ssl
	
# Composer installation
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version="$COMPOSER_VERSION" 

# Install Drush using Composer.
RUN composer global require drush/drush:"$DRUSH_VERSION" --prefer-dist

# MySQL client installation
RUN apt-get update && apt-get install -y mariadb-client && rm -rf /var/lib/apt

# Mailhog
RUN curl --location --output /usr/local/bin/mhsendmail https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 && \
    chmod +x /usr/local/bin/mhsendmail

RUN echo 'sendmail_path="/usr/local/bin/mhsendmail --smtp-addr=mailhog:1025 --from=no-reply@docker.dev"' > /usr/local/etc/php/conf.d/mailhog.ini

# Copy php.ini file to container
COPY /conf/php.ini /usr/local/etc/php/

# Drupal Console installation
RUN curl --location --output /bin/drupal https://drupalconsole.com/installer && chmod +x /bin/drupal

# Install GD
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-install gd

# bz2 extenstion
RUN apt-get update && apt-get install -y libbz2-dev
RUN docker-php-ext-install bz2

# Install PECL Uploadprogress.
RUN pecl install uploadprogress && echo 'extension=uploadprogress.so' > /usr/local/etc/php/conf.d/uploadprogress.ini

# NPM install
RUN apt-get install nodejs -y

# Install soap
RUN docker-php-ext-install soap && docker-php-ext-enable soap
