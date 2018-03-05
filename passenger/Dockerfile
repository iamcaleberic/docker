FROM alpine:edge
LABEL maintainer="iamcaleberic@tuta.io"
ENV PASSENGER_VERSION="5.2.1" \
    PATH="/lib/passenger/bin:$PATH" \
    HOME="/home/app"

# create app user
RUN adduser -h /home/app -G root -D app

RUN PACKAGES="build-base ca-certificates ruby procps curl pcre libstdc++ libexecinfo ruby-bundler ruby-dev bash ruby-etc ruby-rake mariadb-dev ruby-rdoc libpng libpng-dev nodejs yarn pngquant git libffi-dev sqlite-dev ruby-bigdecimal" && \
    BUILD_PACKAGES="linux-headers curl-dev pcre-dev libexecinfo-dev zlib" && \
    apk update && apk add --update $PACKAGES $BUILD_PACKAGES && \
# download and extract
    curl -L https://s3.amazonaws.com/phusion-passenger/releases/passenger-$PASSENGER_VERSION.tar.gz | tar -xzvf - -C /lib && \
    mv /lib/passenger-$PASSENGER_VERSION /lib/passenger && \
# install nginx module
    gem install rack && \
    passenger-install-nginx-module --auto --auto-download --prefix=/etc/nginx --languages 'ruby,nodejs' && \
# Cleanup
    passenger-config validate-install --auto && \
    apk del $BUILD_PACKAGES && \
    rm -rf /var/cache/apk/* \
        /tmp/* \
        /lib/passenger/doc/
# Add nginx config
RUN  mkdir /etc/nginx/conf/sites-available && \
     mkdir /etc/nginx/conf/sites-enabled && \
     echo 'include sites-enabled/*' >> /etc/nginx/conf/nginx.conf

WORKDIR /home/app
EXPOSE 80
