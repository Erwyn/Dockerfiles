FROM carcel/apache-php:latest
MAINTAINER Damien Carcel <damien.carcel@gmail.com>

# Define "root" as current user
USER root

# Deactivate xdebug
RUN phpdismod xdebug

# Install perceptualdiff
RUN apt-get update && \
    apt-get --no-install-recommends --no-install-suggests --yes --quiet install perceptualdiff && \
    apt-get clean && apt-get --yes --quiet autoremove --purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add some utilities
COPY files/pim-in-ass.sh /usr/local/bin/pim-in-ass
COPY files/pim-initialize.sh /usr/local/bin/pim-initialize
COPY files/pim-cac.sh /usr/local/bin/pim-cac
RUN chmod +x /usr/local/bin/*

# Add Akeneo VHOST as default
COPY files/akeneo-behat.local.conf /etc/apache2/sites-available/000-default.conf

# Define "docker" as current user
USER docker
