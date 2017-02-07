FROM python:3.6-alpine
MAINTAINER Justin Tyberg <justin.tyberg@gmail.com>

# Upgrade and install basic OpenStack clients
RUN apk add --no-cache bash \
 && apk add --no-cache --virtual .build-deps \
        python3-dev \
        gcc \
        libc-dev \
        linux-headers \
  && pip install --no-cache-dir \
        python-keystoneclient==3.8.0 \
        python-swiftclient==3.2.0 \
  && apk del .build-deps

CMD ["swift"]
