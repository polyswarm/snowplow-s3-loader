FROM openjdk:11-slim


ARG SBT_VERSION=1.2.8
ARG APP_VERSION=0.8.0

# instal lzo & dpkg
RUN mkdir -p /opt/app && apt update && apt install -y curl lzop liblzo2-dev dpkg

# Install sbt
RUN \
  curl -L -o sbt-$SBT_VERSION.deb https://dl.bintray.com/sbt/debian/sbt-$SBT_VERSION.deb && \
  dpkg -i sbt-$SBT_VERSION.deb && \
  rm sbt-$SBT_VERSION.deb && \
  apt-get update && \
  apt-get install sbt && \
  sbt sbtVersion

WORKDIR /opt/app

COPY . .

RUN sbt assembly

RUN cp target/scala-2.11/snowplow-s3-loader-0.8.0.jar /opt/app && ls -lha

ENTRYPOINT ["java", "-jar", "snowplow-s3-loader-${APP_VERSION}.jar"]

CMD ['--config', 'conf/s3-loader.conf']