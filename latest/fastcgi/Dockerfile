FROM estebanmatias92/hhvm:fastcgi

MAINTAINER "Matias Esteban" <estebanmatias92@gmail.com>

# Install "dockerize" http://github.com/jwilder/dockerize
RUN apt-get update && apt-get install -y wget && rm -rf /var/lib/apt/lists/* \
    && wget --no-check-certificate -O /tmp/dockerize.tar.gz https://github.com/jwilder/dockerize/releases/download/v0.0.2/dockerize-linux-amd64-v0.0.2.tar.gz \
    && tar -C /usr/local/bin -xvzf /tmp/dockerize.tar.gz \
    && rm /tmp/dockerize.tar.gz \
    && apt-get purge -y --auto-remove wget

# Install hhvm extension dependencies
RUN buildDeps=' \
        wget \
        automake \
        autoconf \
        libtool \
        gcc \
    ' \
    && deps=' \
        libpq-dev \
    ' \
    && apt-get update && apt-get install -y $buildDeps $deps && rm -rf /var/lib/apt/lists/* \
    && libbson_version="1.0.0" \
    && wget --no-check-certificate -O /tmp/libbson.tar.gz https://github.com/mongodb/libbson/releases/download/$libbson_version/libbson-$libbson_version.tar.gz \
    && mkdir /tmp/libbson \
    && tar -C /tmp/libbson -xvzf /tmp/libbson.tar.gz --strip-components=! \
    && rm /tmp/libbson.tar.gz \
    && cd /tmp/libbson \
    && ./configure \
    && make \
    && make install \
    && cd - \
    && apt-get purge -y --auto-remove $buildDeps

# Install hhvm extensions
RUN hhvm-ext-install mongofill/mongofill-hhvm PocketRent/hhvm-pgsql

ENTRYPOINT ["/usr/local/bin/hhvm"]

CMD ["--mode", "server"]
