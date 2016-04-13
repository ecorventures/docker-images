FROM anapsix/alpine-java
MAINTAINER Ecor Ventures

# Set environment variables
ENV PKG_NAME elasticsearch
ENV ELASTICSEARCH_VERSION 2.3.1
ENV ELASTICSEARCH_PKG $PKG_NAME-$ELASTICSEARCH_VERSION
ENV ELASTICSEARCH_URL https://download.elasticsearch.org/$PKG_NAME/release/org/$PKG_NAME/distribution/tar/$PKG_NAME/$ELASTICSEARCH_VERSION/$ELASTICSEARCH_PKG.tar.gz

# ADD $ELASTICSEARCH_URL /tmp/$ELASTICSEARCH_PKG.tar.gz

# Download Elasticsearch
RUN apk add -U curl \
    && mkdir -p /opt \
    && curl -o $ELASTICSEARCH_PKG.tar.gz -sSL https://download.elasticsearch.org/$PKG_NAME/release/org/$PKG_NAME/distribution/tar/$PKG_NAME/$ELASTICSEARCH_VERSION/$ELASTICSEARCH_PKG.tar.gz \
    && tar -xzf $ELASTICSEARCH_PKG.tar.gz \
    && rm $ELASTICSEARCH_PKG.tar.gz \
    && mkdir -p /usr/share/elasticsearch \
    && mv $ELASTICSEARCH_PKG /usr/share/elasticsearch \
    && mkdir -p /usr/share/elasticsearch/data /usr/share/elasticsearch/logs /usr/share/elasticsearch/config/scripts \
    && adduser -DH -s /sbin/nologin elasticsearch \
    && chown -R elasticsearch:elasticsearch /usr/share/elasticsearch \
    && apk del curl \
    && rm -rf /var/cache/apk/*

ENV PATH /usr/share/elasticsearch/bin:$PATH

WORKDIR /usr/share/elasticsearch

VOLUME /usr/share/elasticsearch/data

COPY config ./config
COPY entrypoint.sh /

EXPOSE 9200 9300
ENTRYPOINT ["/entrypoint.sh"]
CMD ["elasticsearch"]
