FROM debian:stretch-slim
LABEL maintainer "Anton Kerschbaum <jives@jivesnet.com>"

ARG DEBIAN_FRONTEND=noninteractive
ENV LC_ALL C
ENV GOSU_VERSION 1.9

RUN apt-get update && apt-get install -y \
	ca-certificates \	
	git \
	gnupg \
	libjpeg-dev \
	make \
	openssl \
	python-lxml \
	python-pip \
	wget \ 
	zlib1g-dev \
	&& rm -rf /var/lib/apt/lists/* \
	&& dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')" \
	&& wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch" \
	&& chmod +x /usr/local/bin/gosu \
	&& gosu nobody true

RUN mkdir -p /var/mailpile \
	&& groupadd -g 500 mailpile \
	&& useradd -r -s /bin/false -u 500 -g mailpile -d /var/mailpile mailpile \
	&& chown mailpile: /var/mailpile

RUN git clone https://github.com/mailpile/Mailpile.git \
	&& cd Mailpile \
	&& pip install -r requirements.txt \
	&& ./mp setup

EXPOSE 33411

CMD gosu mailpile /Mailpile/mp --www=0.0.0.0:33411/mailpile/ --wait
