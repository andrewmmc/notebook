FROM jupyter/base-notebook

USER root

COPY install-composer.sh /tmp

RUN pip install -U requests \
                    pyquery \
                    shapely \
                    ipywidgets

RUN cd /tmp \
    && apt-get update \
    && apt-get install -yq --no-install-recommends \
        apt-utils \
        curl \
        pkg-config \
        software-properties-common \
    && LANG=C.UTF-8 add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -yq --no-install-recommends \
        libzmq-dev \
        php-pear \
        php7.2-bcmath \
        php7.2-bz2 \
        php7.2-cli \
        php7.2-common \
        php7.2-curl \
        php7.2-dev \
        php7.2-gd \
        php7.2-gmp \
        php7.2-intl \
        php7.2-json \
        php7.2-mbstring \
        php7.2-mysql \
        php7.2-zip \
    && yes '' | pecl install zmq-beta \
    && echo "extension=zmq.so" > /etc/php/7.2/mods-available/zmq.ini \
    && ln -s /etc/php/7.2/mods-available/zmq.ini /etc/php/7.2/cli/conf.d/30-zmq.ini \
    && chmod 0755 install-composer.sh && ./install-composer.sh \
    && chmod 0755 composer.phar && mv composer.phar /usr/bin/composer \
    && curl -s -L -O "https://litipk.github.io/Jupyter-PHP-Installer/dist/jupyter-php-installer.phar" \
    && curl -s -L -O "https://litipk.github.io/Jupyter-PHP-Installer/dist/jupyter-php-installer.phar.sha512" \
    && shasum -s -a 512 -c jupyter-php-installer.phar.sha512 \
    && php jupyter-php-installer.phar install \
    && rm -rf /tmp/*

USER $NB_USER

RUN python --version

RUN php --version

WORKDIR /home/jovyan