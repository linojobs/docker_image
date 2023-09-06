FROM alpine

EXPOSE 9000

ADD php-8.2.4.tar.gz /
ADD xdebug-3.2.2.tar.gz /

RUN apk add --no-cache gcc g++ automake autoconf libtool make linux-headers libxml2-dev libzip-dev

RUN cd /php-8.2.4 && rm -fr ./configure && ./buildconf --force && ./configure \
    --prefix=/usr/local/php \
    --enable-mysqlnd \
    --with-mysqli \
    --with-pear \
    --with-zip \
    --with-pdo-mysql \
    --without-pdo-sqlite \
    --disable-mbregex \
    --without-sqlite3 \
    --disable-short-tags \
    --enable-fpm \
    --without-iconv \
    --disable-phar \
    --enable-ds \
    --enable-bcmath \
    --enable-pcntl \
    --enable-phalcon \
    && make \
    && make install

RUN cd /xdebug-3.2.2 && /usr/local/php/bin/phpize && ./configure --enable-xdebug \
    --with-php-config=/usr/local/php/bin/php-config \
    && make \
    && make install

RUN mv /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
RUN ln -s /usr/local/php/bin/php /usr/bin/php
COPY php.ini /usr/local/php/lib/php.ini
COPY www.conf /usr/local/php/etc/php-fpm.d

RUN rm -fr php-8.2.4
RUN rm -fr xdebug-3.2.2

CMD [ "/usr/local/php/sbin/php-fpm","-c","/usr/local/php/lib","-F"]
