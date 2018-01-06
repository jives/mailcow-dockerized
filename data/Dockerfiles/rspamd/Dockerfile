FROM debian:stretch-slim

ARG DEBIAN_FRONTEND=noninteractive
ENV LC_ALL C
ENV RSPAMD_RUN=/run/rspamd
ENV RSPAMD_HOME=/var/lib/rspamd
ENV RSPAMD_LOG=/var/log/rspamd
ENV RSPAMD_USER=_rspamd

RUN apt-get update && apt-get install -y \
	automake \
	autotools-dev \
	build-essential \
	ca-certificates \
	cmake \
	git \
	libevent-dev \
	libfann-dev \
	libglib2.0-dev \
	libicu-dev \
	liblua5.1-dev \
	libmagic-dev \
	libsqlite3-dev \
	libssl-dev \
	lua5.1 \
	make \
	openssl \
	ragel \
	sqlite3 \
	&& rm -rf /var/lib/apt/lists/*

RUN git clone --recursive https://github.com/vstakhov/rspamd.git \
	&& mkdir rspamd.build && cd rspamd.build \
	&& cmake ../rspamd \
  		-DCMAKE_INSTALL_PREFIX=/usr \
  		-DCMAKE_VERBOSE_MAKEFILE=ON \
  		-DCMAKE_BUILD_TYPE=None \
  		-DCMAKE_INSTALL_SYSCONFDIR=/etc \
  		-DCMAKE_INSTALL_LOCALSTATEDIR=/var \
  		-DCONFDIR=/etc/rspamd \
  		-DMANDIR=/usr/share/man \
  		-DRUNDIR=/run/rspamd \
  		-DDBDIR=/var/lib/rspamd \
  		-DLOGDIR=/var/log/rspamd \
  		-DPLUGINSDIR=/usr/share/rspamd \
  		-DEXAMPLESDIR=/usr/share/doc/rspamd/examples \
  		-DLIBDIR=/usr/lib/rspamd \
  		-DINCLUDEDIR=/usr/include \
  		-DSYSTEMDDIR=/lib/systemd/system \
	&& make -j4 \
	&& make install \
	&& make clean \
	&& cd .. && rm -rf rspamd rspamd.build

RUN adduser --quiet \
	--system \
	--group \
	--home ${RSPAMD_HOME} \
	--no-create-home \
	--disabled-login \
	--gecos "rspamd spam filtering system" \
	--force-badname \
	${RSPAMD_USER} \
	&& mkdir -p ${RSPAMD_HOME} ${RSPAMD_LOG} ${RSPAMD_RUN} \
	&& chown ${RSPAMD_USER}: ${RSPAMD_HOME} ${RSPAMD_LOG} ${RSPAMD_RUN} 

COPY settings.conf /etc/rspamd/modules.d/settings.conf
#COPY ratelimit.lua /usr/share/rspamd/lua/ratelimit.lua
#COPY lua_util.lua /usr/share/rspamd/lib/lua_util.lua
COPY docker-entrypoint.sh /docker-entrypoint.sh
COPY tini-arm64 /sbin/tini

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["/usr/bin/rspamd", "-f", "-u", "_rspamd", "-g", "_rspamd"]
