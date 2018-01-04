FROM nginx:mainline

MAINTAINER Anton Kerschbaum

RUN apt-get update && apt-get -y install \
	iputils-ping \
	&& rm -rf /var/lib/apt/lists/* \
	&& apt-get autoremove --purge \
	&& apt-get clean

CMD ["nginx"]
