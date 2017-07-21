FROM cmp1234/node:6.11.1

# add our user and group first to make sure their IDs get assigned consistently
RUN adduser -D -H kibana


ENV KIBANA_MAJOR 4.6
ENV KIBANA_VERSION 4.6.4

RUN set -x \
    	&& apk add --no-cache --virtual .build-deps \
		curl \
	&& KIBANA_HOME_BASE=/usr/local \
	&& KIBANA_HOME=$KIBANA_HOME_BASE/kibana \
	&& curl -fSL https://download.elastic.co/kibana/kibana/kibana-${KIBANA_VERSION}-linux-x86_64.tar.gz -o kibana.tar.gz \
	&& tar xf kibana.tar.gz -C $KIBANA_HOME_BASE \
	&& mv $KIBANA_HOME_BASE/kibana-${KIBANA_VERSION}-linux-x86_64 $KIBANA_HOME_BASE/kibana \
	&& rm -f $KIBANA_HOME/node/bin/* \
	&& rm -f kibana.tar.gz \
	&& chown -R kibana:kibana $KIBANA_HOME \
	&& sed -ri "s!^(\#\s*)?(elasticsearch\.url:).*!\2 'http://elasticsearch:9200'!" $KIBANA_HOME/config/kibana.yml \
	&& grep -q 'elasticsearch:9200' $KIBANA_HOME/config/kibana.yml \
	&& apk del .build-deps

ENV PATH /usr/local/kibana/bin:$PATH

COPY docker-entrypoint.sh /

EXPOSE 5601
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["kibana"]
