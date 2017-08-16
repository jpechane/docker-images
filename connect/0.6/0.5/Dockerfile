FROM debezium/connect-base:0.5

MAINTAINER Debezium Community

ENV DEBEZIUM_VERSION=0.5.2 \
    MAVEN_CENTRAL="https://repo1.maven.org/maven2" \
    MD5SUMS="MONGODB_MD5=ae582efdb1d825ed696646dcd81c760f MYSQL_MD5=00c1c35a8b75cc96a8684958056cfc5d POSTGRES_MD5=f9f2f3a3a5f1fc728e889e5ab798ae4b"

#
# Create a single `$KAFKA_CONNECT_PLUGINS_DIR/debezium` directory into which we'll place all of our JARs and files.
#
# Debezium connectors share some dependencies and JARs, so if we put each connector into a separate directory
# then we'd have JARs appearing in multiple places on Kafka Connect's flat classpath, and we'd get 
# NoSuchMethod exceptions.
RUN mkdir $KAFKA_CONNECT_PLUGINS_DIR/debezium

#
# Download connectors, verify the contents, and then install into the `$KAFKA_CONNECT_PLUGINS_DIR/debezium` directory...
#
RUN eval $MD5SUMS &&\
    for CONNECTOR in {mysql,mongodb,postgres}; do \
    curl -fSL -o /tmp/plugin.tar.gz \
                 $MAVEN_CENTRAL/io/debezium/debezium-connector-$CONNECTOR/$DEBEZIUM_VERSION/debezium-connector-$CONNECTOR-$DEBEZIUM_VERSION-plugin.tar.gz &&\
    declare MD5_PARAM_NAME="${CONNECTOR^^}_MD5" &&\
    echo "${!MD5_PARAM_NAME}  /tmp/plugin.tar.gz" | md5sum -c - &&\
    tar -xzf /tmp/plugin.tar.gz -C $KAFKA_CONNECT_PLUGINS_DIR/debezium --strip 1 &&\
    rm -f /tmp/plugin.tar.gz; \
    done;
