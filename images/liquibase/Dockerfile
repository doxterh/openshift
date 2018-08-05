FROM openjdk:8

ENV LIQUIBASE_VERSION="3.6.2" \
    LIQUIBASE_DRIVER="org.postgresql.Driver" \
    LIQUIBASE_CLASSPATH="/usr/share/java/postgresql.jar" \
    LIQUIBASE_URL="" \
    LIQUIBASE_USERNAME="" \
    LIQUIBASE_PASSWORD="" \
    LIQUIBASE_CHANGELOG="liquibase.xml" \
    LIQUIBASE_CONTEXTS="" \
    LIQUIBASE_OPTS=""

COPY bin/entrypoint.sh /

RUN set -x \
    && apt-get update \
	&& apt-get install -yq --no-install-recommends \
		libpostgresql-jdbc-java \
    && wget -q -O/tmp/liquibase.tar.gz "https://github.com/liquibase/liquibase/releases/download/liquibase-parent-${LIQUIBASE_VERSION}/liquibase-${LIQUIBASE_VERSION}-bin.tar.gz" \
    && useradd -ms /bin/bash liquibase \
    && mkdir -p /opt/liquibase \
    && mkdir -p /liquibase \
    && tar -xzf /tmp/liquibase.tar.gz -C /opt/liquibase \
    && rm -f /tmp/liquibase.tar.gz \
    && chown -R liquibase:liquibase /opt/liquibase /liquibase /entrypoint.sh \
    && chmod +x /opt/liquibase/liquibase \
    && ln -s /opt/liquibase/liquibase /usr/local/bin/ \
    && chmod +x /entrypoint.sh \
    && touch /liquibase.properties  \
    && chown -R liquibase:liquibase /liquibase.properties \
    && wget -q -O /opt/liquibase/lib/slf4j-api-1.7.25.jar "http://central.maven.org/maven2/org/slf4j/slf4j-api/1.7.25/slf4j-api-1.7.25.jar" \
	&& apt-get clean \
	&& rm -r /var/lib/apt/lists/* \
    && apt-get autoremove -y -f \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

USER liquibase

WORKDIR /liquibase
ENTRYPOINT ["/entrypoint.sh"]