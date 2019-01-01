FROM microsoft/mssql-server-linux

ENV FLYWAY_VERSION 5.2.4
RUN apt-get update \
    && apt-get install -y --no-install-recommends curl \
    && curl -L https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/${FLYWAY_VERSION}/flyway-commandline-${FLYWAY_VERSION}.tar.gz -o flyway-commandline.tar.gz \
    && mkdir -p /flyway \
    && tar -xzf flyway-commandline.tar.gz -C /flyway --strip-components=1 \
    && rm flyway-commandline.tar.gz \
    && ln -s /flyway/flyway /usr/local/bin/flyway \
    && apt-get remove -y curl \
    && apt-get install -y --no-install-recommends openjdk-8-jre
