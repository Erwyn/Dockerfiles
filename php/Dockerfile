FROM debian:jessie
MAINTAINER Damien Carcel <damien.carcel@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

# Install PHP with some extensions
RUN apt-get update && \
    apt-get --no-install-recommends --no-install-suggests --yes --quiet install \
        ca-certificates bash-completion curl git imagemagick vim wget sudo && \
    apt-get clean && apt-get --yes --quiet autoremove --purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add PHP 5.4
COPY files/wheezy.list /etc/apt/sources.list.d/wheezy.list
COPY files/php5.preferences /etc/apt/preferences.d/php5
RUN apt-get update && \
    apt-get --no-install-recommends --no-install-suggests --yes --quiet install \
        php-pear build-essential php5-dev php5-common \
        php5-cli php-apc php5-curl php5-gd php5-imagick php5-intl php5-mcrypt php5-mysql && \
    apt-get clean && apt-get --yes --quiet autoremove --purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Instal PHP mongo extension
RUN pecl install mongo && \
    echo "extension=mongo.so" | sudo tee /etc/php5/conf.d/mongo.ini > /dev/null

# Add a "docker" user
RUN useradd docker --shell /bin/bash --create-home \
  && usermod --append --groups sudo docker \
  && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
  && echo 'docker:secret' | chpasswd

# Install composer
RUN curl -sSL https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN chmod +x /usr/local/bin/composer

# Configure PHP
RUN php5enmod mcrypt

RUN sed -i "s/;date.timezone =/date.timezone = Europe\/Paris/" /etc/php5/cli/php.ini && \
    sed -i "s/memory_limit = .*/memory_limit = 2G/" /etc/php5/cli/php.ini && \
    sed -i "s/upload_max_filesize = .*/upload_max_filesize = 20M/" /etc/php5/cli/php.ini && \
    sed -i "s/post_max_size = .*/post_max_size = 20M/" /etc/php5/cli/php.ini

# Expose Xdebug
EXPOSE 9000

# Define "docker" as current user
USER docker
WORKDIR /home/docker/
