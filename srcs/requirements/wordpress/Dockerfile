FROM debian:bullseye

ARG MYSQL_HOST
ARG MYSQL_DATABASE
ARG MYSQL_USER
ARG MYSQL_PASSWORD

RUN apt-get update && apt-get install -y \
	php7.4 \
	php7.4-fpm \
	php7.4-mysql \
	php7.4-gd \
	php7.4-opcache \
	php7.4-cli \
	php7.4-curl \
	php7.4-mbstring \
	wget \
	apt-utils \
	unzip

RUN wget -P /tmp https://wordpress.org/latest.zip \
	&& unzip /tmp/latest.zip -d /var/www/ \
	&& rm /tmp/latest.zip

RUN wget -P /tmp https://downloads.wordpress.org/theme/twentyseventeen.3.2.zip \
	&& unzip /tmp/twentyseventeen.3.2.zip -d /var/www/wordpress/wp-content/themes/ \
	&& rm /tmp/twentyseventeen.3.2.zip

COPY ./conf/www.conf /etc/php/7.4/fpm/pool.d/

RUN chown -R www-data:www-data /var/www/wordpress \
	&& chmod -R 777 /var/www/wordpress

RUN mkdir /run/php

WORKDIR /var/www/wordpress/

RUN cp wp-config-sample.php wp-config.php && \
	mv wp-config-sample.php /var/www/wordpress/wp-config.php && \
	sed -i "s/username_here/$MYSQL_USER/g" /var/www/wordpress/wp-config.php && \
	sed -i "s/password_here/$MYSQL_PASSWORD/g" /var/www/wordpress/wp-config.php && \
	sed -i "s/localhost/$MYSQL_HOST/g" /var/www/wordpress/wp-config.php && \
	sed -i "s/database_name_here/$MYSQL_DATABASE/g" /var/www/wordpress/wp-config.php && \
	chown -R www-data:www-data /var/www/wordpress

EXPOSE 9000

CMD ["php-fpm7.4", "-F"]