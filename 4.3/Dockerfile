FROM montge/centos-java8

# add our user and group first to make sure their IDs get assigned consistently
RUN groupadd -r kibana && useradd -r -m -g kibana kibana

# Install gosu - https://github.com/tianon/gosu
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4
RUN set -x
RUN curl -L https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64 -o /usr/local/sbin/gosu;
RUN curl -L https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64.asc -o /usr/local/sbin/gosu.asc;
RUN gpg --verify /usr/local/sbin/gosu.asc
RUN rm /usr/local/sbin/gosu.asc
RUN chmod 0755 /usr/local/sbin/gosu

ENV KIBANA_VERSION 4.3.0
ENV KIBANA_SHA1 d64e1fc0ddeaaab85e168177de6c78ed82bb3a3b

RUN set -x \
	&& curl -fSL "https://download.elastic.co/kibana/kibana/kibana-${KIBANA_VERSION}-linux-x64.tar.gz" -o kibana.tar.gz \
	&& echo "${KIBANA_SHA1}  kibana.tar.gz" | sha1sum -c - \
	&& mkdir -p /opt/kibana \
	&& tar -xz --strip-components=1 -C /opt/kibana -f kibana.tar.gz \
	&& chown -R kibana:kibana /opt/kibana \
	&& rm kibana.tar.gz

ENV PATH /opt/kibana/bin:$PATH

COPY ./docker-entrypoint.sh /

EXPOSE 5601
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["kibana"]
