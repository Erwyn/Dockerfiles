FROM carcel/fpm:5.6
MAINTAINER Damien Carcel <damien.carcel@gmail.com>

# Define "root" as current user
USER root

# Install perceptualdiff
RUN apt-get update && \
    apt-get --no-install-recommends --no-install-suggests -yq install perceptualdiff && \
    apt-get clean && apt-get -yq autoclean && apt-get -yq autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add some utilities
ADD files/pim-in-ass.sh /usr/local/bin/pim-in-ass
ADD files/pim-initialize.sh /usr/local/bin/pim-initialize
RUN chmod +x /usr/local/bin/*

# Configure PHP FPM
RUN sed -i "s/listen = .*/listen = 9001/" /etc/php5/fpm/pool.d/www.conf

# Expose PHP FPM
EXPOSE 9001

# Define "docker" as current user
USER docker