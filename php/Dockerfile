FROM php:7.3-fpm
LABEL maintainer="Aaron Bell <aaron.bell@swipeandtap.co.uk>"
## Install Git and others dependencies
RUN apt-get update && apt-get install -y \
    openssl \
    git \
    unzip \
    wget \
    xvfb \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    gnupg2\
    libzip-dev \
    zip \
    libmagickwand-dev \
  && docker-php-ext-configure zip --with-libzip \
  && docker-php-ext-install zip \
  && pecl install imagick \
  && docker-php-ext-enable imagick
## Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer --version
## Install MYSQL driver
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/  &&  \
    docker-php-ext-install pdo pdo_mysql gd
## Install Node and tools
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g \
    grunt-cli \
    gulp-cli \
    bower

## Install Yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - &&\
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update && apt-get install yarn

# Set up the Chrome PPA
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN dpkg -i google-chrome-stable_current_amd64.deb; apt-get -fy install

# Set up Chromedriver Environment variables
ENV CHROMEDRIVER_VERSION 2.33
ENV CHROMEDRIVER_DIR /chromedriver
RUN mkdir $CHROMEDRIVER_DIR
# Download and install Chromedriver
RUN wget -q --continue -P $CHROMEDRIVER_DIR "http://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip" \
    && unzip $CHROMEDRIVER_DIR/chromedriver* -d $CHROMEDRIVER_DIR
# Put Chromedriver into the PATH
ENV PATH $CHROMEDRIVER_DIR:$PATH
# Install PHPUnit
RUN wget https://phar.phpunit.de/phpunit-8.5.2.phar \
    && chmod +x phpunit-8.5.2.phar \
    && mv phpunit-8.5.2.phar /usr/local/bin/phpunit

WORKDIR /var/www