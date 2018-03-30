FROM php:7.2-fpm
MAINTAINER goozp "gzp@goozp.com"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        git \
        vim \
        curl \
        wget \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
		libssl-dev \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && rm -r /var/lib/apt/lists/*

# 将预先下载好的拓展包从宿主机拷贝进去
COPY ./pkg/redis.tgz /home/redis.tgz
COPY ./pkg/libmemcached.tar.gz /home/libmemcached.tar.gz
COPY ./pkg/memcached.tgz /home/memcached.tgz

# 安装 PECL 拓展，这里我们安装的是Redis
RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# 安装libmemcached
RUN tar zxvf /home/libmemcached.tar.gz \
    && cd libmemcached-1.0.18 \
    && ./configure --prefix=/usr/local/libmemcached --with-memcached \
    && make && make install

# 安装memcached
RUN tar zxvf /home/memcached.tgz \
    && cd memcached-3.0.4 \
    && phpize \
    && ./configure -enable-memcached -with-php-config=/usr/local/bin/php-config -with-zlib-dir -with-libmemcached-dir=/usr/local/libmemcached -prefix=/usr/local/phpmemcached -disable-memcached-sasl \
    && make && make install \
    && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/memcached.ini

# 安装 Swoole
RUN cd /home \
    && git clone https://gitee.com/swoole/swoole.git \
    && cd swoole \
    && phpize \
    && ./configure --enable-openssl -with-php-config=/usr/local/bin/php-config \
    && make \
    && make install \
    && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

# 删除扩展包
RUN rm -f /home/redis.tgz
RUN rm -f /home/libmemcached.tar.gz
RUN rm -f /home/memcached.tgz

WORKDIR /data

# 写权限
RUN usermod -u 1000 www-data
