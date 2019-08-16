FROM debezium/connect-base:0.10

LABEL maintainer="Debezium Community"

ENV DEBEZIUM_VERSION="0.10.0.Beta4" \
    MAVEN_REPO_CENTRAL="" \
    MAVEN_REPO_INCUBATOR="" \
    MAVEN_DEP_DESTINATION=$KAFKA_CONNECT_PLUGINS_DIR \
    MONGODB_MD5=87b0f7515cbf4ac809060b80f5f7e2df \
    MYSQL_MD5=1cd3c6c58657b2748024ed45d8851758 \
    POSTGRES_MD5=b00f2034f54c682ea95353e23d090e25 \
    SQLSERVER_MD5=d976b746a89c53efc05cad72fd615a60 \
    ORACLE_MD5=f7cc6ff4e90308c95ebabff4621b742e

RUN docker-maven-download debezium mongodb "$DEBEZIUM_VERSION" "$MONGODB_MD5" && \
    docker-maven-download debezium mysql "$DEBEZIUM_VERSION" "$MYSQL_MD5" && \
    docker-maven-download debezium postgres "$DEBEZIUM_VERSION" "$POSTGRES_MD5" && \
    docker-maven-download debezium sqlserver "$DEBEZIUM_VERSION" "$SQLSERVER_MD5" && \
    docker-maven-download debezium-incubator oracle "$DEBEZIUM_VERSION" "$ORACLE_MD5"
