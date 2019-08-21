FROM debian:buster-slim

ARG smokeping_ver
ARG DEBIAN_FRONTEND=noninteractive
ARG TZ=Australia/Sydney

RUN apt-get update
RUN apt-get install --no-install-recommends -y \
build-essential \
ca-certificates \
curl \
dnsutils \
echoping \
fping \
librrds-perl \
libssh-dev \
openssh-client \
rrdtool \
spawn-fcgi \
tini \
wget

RUN wget --no-verbose --tries=5 --timeout=5 \
  https://oss.oetiker.ch/smokeping/pub/smokeping-$smokeping_ver.tar.gz -O /tmp/smokeping.tar.gz

RUN mkdir /tmp/smokeping && \
  tar -xf /tmp/smokeping.tar.gz --directory /tmp/smokeping --strip 1

RUN cd /tmp/smokeping && \
  ./configure --prefix=/opt/smokeping && \
  make install

RUN ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime && \
  dpkg-reconfigure -f noninteractive tzdata

WORKDIR /app

ENTRYPOINT [ "/usr/bin/tini", "--" ]

CMD [ "/usr/bin/spawn-fcgi", "-n", "-p", "9000", "--", "/opt/smokeping/htdocs/smokeping.fcgi" ]
