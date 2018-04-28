FROM jupyter/base-notebook
MAINTAINER Andrew Mok <hello@andrewmmc.com>

USER root

COPY install-composer.sh /tmp

## Install Python related dependencies
RUN pip install -U requests pyquery shapely ipywidgets

## Update package and Install common dependencies
RUN apt-get update && apt-get install -y software-properties-common \
        apt-utils curl pkg-config \
    && LANG=C.UTF-8 add-apt-repository -y ppa:ondrej/php \
    && apt-get update

## Install PHP related dependencies
RUN apt-get install -y libzmq-dev \
        php-pear \
        php7.2-bcmath php7.2-bz2 php7.2-cli \
        php7.2-common php7.2-curl php7.2-dev \
        php7.2-gd php7.2-gmp php7.2-intl \
        php7.2-json php7.2-mbstring php7.2-mysql \
        php7.2-zip \
    && yes '' | pecl install zmq-beta \
    && echo "extension=zmq.so" > /etc/php/7.2/mods-available/zmq.ini \
    && ln -s /etc/php/7.2/mods-available/zmq.ini /etc/php/7.2/cli/conf.d/30-zmq.ini

RUN cd /tmp && chmod 0755 install-composer.sh && ./install-composer.sh \
    && chmod 0755 composer.phar && mv composer.phar /usr/bin/composer

RUN curl -L -O "https://litipk.github.io/Jupyter-PHP-Installer/dist/jupyter-php-installer.phar" \
    && php jupyter-php-installer.phar install \
    && rm -rf /tmp/*

## Install Node.js related dependencies

RUN apt-get install -y nodejs-legacy npm

USER $NB_USER

RUN npm install -g ijavascript

CMD ijs --ip=* --debug

RUN python --version && php --version && node --version

WORKDIR /home/jovyan