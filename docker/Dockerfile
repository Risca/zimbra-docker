#################################################################
# Dockerfile to build Zimbra Collaboration 8.8.7 container images
# Based on Ubuntu 16.04
# Created by Jorge de la Cruz
#################################################################
FROM ubuntu:bionic
MAINTAINER Jorge de la Cruz <jorgedlcruz@gmail.com>

RUN echo "resolvconf resolvconf/linkify-resolvconf boolean false" | debconf-set-selections

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install \
  wget \
  dialog \
  openssh-client \
  openssh-server \
  software-properties-common \
  dnsutils \
  locales \
  iproute2 \
  net-tools \
  python3 \
  python3-venv \
  libaugeas0 \
  sudo \
  rsyslog \
  unzip \
  && locale-gen en_US en_US.UTF-8 en_GB en_GB.UTF-8 \
  && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
  && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

RUN python3 -m venv /opt/certbot/ \
  && /opt/certbot/bin/pip install --upgrade pip \
  && /opt/certbot/bin/pip install certbot \
  && ln -s /opt/certbot/bin/certbot /usr/bin/certbot
RUN echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && /usr/bin/certbot renew -q" | tee -a /etc/crontab

EXPOSE 22 25 465 587 110 143 993 995 80 443 8080 8443 7071

COPY opt /opt/

COPY etc /etc/

CMD ["/bin/bash", "/opt/start.sh", "-d"]
