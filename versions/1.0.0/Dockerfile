FROM nginx:alpine

EXPOSE 443

# Prime container with self sign first, this container later will get real certs
# with the shared volume from a letsencrypt container.
RUN mkdir -p /etc/nginx/certs/ && \
    mkdir -p /etc/nginx/html/certbot/.well-known/acme-challenge/

RUN apk update && apk add openssl --no-cache
RUN openssl req -x509 \
      -nodes \
      -days 365 \
      -newkey rsa:2048 \
      -keyout /etc/nginx/certs/privkey.pem \
      -out /etc/nginx/certs/fullchain.pem \
      -subj "/C=US/ST=DE/L=DE/O=hi-five.us/OU=Org/CN=site.test/emailAddress=selfsign"

RUN openssl dhparam -out /etc/nginx/certs/dhparam.pem 2048 > /dev/null 2>&1

#
# -- Volumes Last --
#

VOLUME /etc/nginx/certs
VOLUME /etc/nginx/conf.d
VOLUME /etc/nginx/html/certbot
