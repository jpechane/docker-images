FROM debezium/connect-base:0.5

MAINTAINER Debezium Community

ENV DEBEZIUM_VERSION=0.5.2 \
    MAVEN_CENTRAL="https://repo1.maven.org/maven2" \
    MD5SUMS="MONGODB_MD5=e10eb11420b25f1100d7d9e33372dd78 MYSQL_MD5=c8b539b6d3dd230c464fe171431f8f46 POSTGRES_MD5=1bbc364ecce7fbb4375e2da1b0efad2c"

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
