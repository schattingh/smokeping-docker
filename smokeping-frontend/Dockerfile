FROM nginx:mainline

ARG smokeping_ver
ARG DEBIAN_FRONTEND=noninteractive
ARG TZ=Australia/Sydney

RUN apt-get update
RUN apt-get install --no-install-recommends -y \
  ca-certificates \
  curl \
  wget

RUN wget --no-verbose --tries=5 --timeout=5 \
  https://oss.oetiker.ch/smokeping/pub/smokeping-$smokeping_ver.tar.gz -O /tmp/smokeping.tar.gz

RUN mkdir /opt/smokeping && \
  tar -xf /tmp/smokeping.tar.gz --directory /opt/smokeping --strip 1

RUN ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime && \
  dpkg-reconfigure -f noninteractive tzdata
