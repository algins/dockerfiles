FROM php:8.2.0-fpm

# Environment variables
ENV DEBIAN_FRONTEND noninteractive
ENV LD_LIBRARY_PATH /opt/oracle/instantclient_12_1:${LD_LIBRARY_PATH}

# Packages
RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        wget \
        git \
        curl \
        unzip \
        gnupg \
        apt-transport-https

# Software repository for Microsoft products
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list

# Oracle Instantclient
RUN mkdir /opt/oracle \
	&& cd /opt/oracle 
ADD instantclient-basic-linux.x64-12.1.0.2.0.zip /opt/oracle
ADD instantclient-sdk-linux.x64-12.1.0.2.0.zip /opt/oracle
RUN  unzip /opt/oracle/instantclient-basic-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
    && unzip /opt/oracle/instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
    && ln -s /opt/oracle/instantclient_12_1/libclntsh.so.12.1 /opt/oracle/instantclient_12_1/libclntsh.so \
    && ln -s /opt/oracle/instantclient_12_1/libclntshcore.so.12.1 /opt/oracle/instantclient_12_1/libclntshcore.so \
    && ln -s /opt/oracle/instantclient_12_1/libocci.so.12.1 /opt/oracle/instantclient_12_1/libocci.so \
	&& rm -rf /opt/oracle/*.zip

# PHP extension dependencies
RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        libldap2-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        openssl \
        unixodbc-dev \
        libaio1

RUN ACCEPT_EULA=Y apt-get install -y msodbcsql17 mssql-tools

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/local/bin \
    --filename=composer

# PHP extensions
RUN echo 'instantclient,/opt/oracle/instantclient_12_1/' | pecl install \
        oci8 \
        sqlsrv \
        pdo_sqlsrv \
    && docker-php-ext-install \
        bcmath \
        ctype \
        fileinfo \
        ldap \
        mbstring \
        pdo \
        soap \
        xml \
        zip \
    && docker-php-ext-enable \
        oci8 \
        sqlsrv \
        pdo_sqlsrv \
    && docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient_12_1,12.1 \
       && docker-php-ext-install pdo_oci

# Cleanup
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# workaround for https://github.com/microsoft/msphpsql/issues/1021
RUN sed -i -E 's/(CipherString\s*=\s*DEFAULT@SECLEVEL=)2/\11/' /etc/ssl/openssl.cnf

RUN usermod -u 1000 www-data

CMD ["php-fpm"]

EXPOSE 9000