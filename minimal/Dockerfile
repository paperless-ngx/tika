
# Licensed under the Apache License, Version 2.0 (the "License"); you may not
# use this file except in compliance with the License. You may obtain a copy of
# the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
# License for the specific language governing permissions and limitations under
# the License.
FROM ubuntu:focal as base
RUN apt-get update

FROM base as dependencies

# "random" uid/gid hopefully not used anywhere else
ARG UID_GID="35002:35002"
ARG JRE='openjdk-17-jre-headless'

RUN DEBIAN_FRONTEND=noninteractive apt-get -y install $JRE gnupg2 wget ca-certificates

FROM dependencies as fetch_tika
ARG TIKA_VERSION
ARG TIKA_JAR_NAME=tika-server
ARG CHECK_SIG=true

ENV ARCHIVE_TIKA_SERVER_URL="https://archive.apache.org/dist/tika/${TIKA_VERSION}/${TIKA_JAR_NAME}-${TIKA_VERSION}.jar" \
    ARCHIVE_TIKA_SERVER_ASC_URL="https://archive.apache.org/dist/tika/${TIKA_VERSION}/${TIKA_JAR_NAME}-${TIKA_VERSION}.jar.asc" \
    TIKA_VERSION=$TIKA_VERSION

RUN wget -t 10 --no-check-certificate --max-redirect 1 --retry-connrefused $ARCHIVE_TIKA_SERVER_URL -O /${TIKA_JAR_NAME}-${TIKA_VERSION}.jar
RUN wget -t 10 --no-check-certificate --max-redirect 1 --retry-connrefused $ARCHIVE_TIKA_SERVER_ASC_URL -O /${TIKA_JAR_NAME}-${TIKA_VERSION}.jar.asc || rm /${TIKA_JAR_NAME}-${TIKA_VERSION}.jar.asc 

RUN if [ "$CHECK_SIG" = "true" ] ; then \
        wget -t 10 --no-check-certificate --max-redirect 1 --retry-connrefused -q -O keys.gpg https://downloads.apache.org/tika/KEYS \
        gpg --import keys.gpg \
        gpg --verify /${TIKA_JAR_NAME}-${TIKA_VERSION}.jar.asc /${TIKA_JAR_NAME}-${TIKA_VERSION}.jar; \
    fi

FROM dependencies as runtime
RUN apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
ARG TIKA_VERSION
ENV TIKA_VERSION=$TIKA_VERSION
ARG TIKA_JAR_NAME
ENV TIKA_JAR_NAME=$TIKA_JAR_NAME
COPY --from=fetch_tika /${TIKA_JAR_NAME}-${TIKA_VERSION}.jar /${TIKA_JAR_NAME}-${TIKA_VERSION}.jar
USER $UID_GID
EXPOSE 9998
ENTRYPOINT [ "/bin/sh", "-c", "exec java -jar /${TIKA_JAR_NAME}-${TIKA_VERSION}.jar -h 0.0.0.0 $0 $@"]

LABEL maintainer="Apache Tika Developers dev@tika.apache.org"
