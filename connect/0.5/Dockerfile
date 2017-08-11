FROM debezium/connect-base:0.5

MAINTAINER Debezium Community

ENV DEBEZIUM_VERSION=0.5.2 \
    MAVEN_CENTRAL="https://repo1.maven.org/maven2" \
    MD5SUMS="MONGODB_MD5=5670d36457ae9a5184cbcebb8d88b02f MYSQL_MD5=c7026dd3434b77ff3637e71c7dccbc63 POSTGRES_MD5=09e013f6400194cb7db6f3300e25db16"

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
