FROM mariadb:10

MAINTAINER Debezium Community

COPY mysql.cnf /etc/mysql/mariadb.conf.d/
COPY inventory.sql /docker-entrypoint-initdb.d/
