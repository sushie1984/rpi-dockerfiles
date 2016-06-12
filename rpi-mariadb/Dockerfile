FROM resin/rpi-raspbian:jessie

MAINTAINER Sascha Burku Burku <sascha_burku@yahoo.de>

RUN groupadd -r mysql && useradd -r -g mysql mysql

RUN mkdir /docker-entrypoint-initdb.d

ENV MARIADB_VERSION 10.0.25-0+deb8u1

RUN apt-get update && apt-get install -y mariadb-server="${MARIADB_VERSION}"* \
   && rm -rf /var/lib/apt/lists/* && rm -rf /var/lib/mysql && mkdir -p /var/lib/mysql

RUN sed -Ei 's/^(bind-address|log)/#&/' /etc/mysql/my.cnf \
	&& echo 'skip-host-cache\nskip-name-resolve' | awk '{ print } $1 == "[mysqld]" && c == 0 { c = 1; system("cat") }' /etc/mysql/my.cnf > /tmp/my.cnf \
	&& mv /tmp/my.cnf /etc/mysql/my.cnf

VOLUME /var/lib/mysql

COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 3306

CMD ["mysqld"]
