FROM --platform=$BUILDPLATFORM curlimages/curl:7.81.0 AS jar
ARG BUILDPLATFORM
ARG VERSION
WORKDIR /tmp
RUN curl -f -k -O https://colossus.kruemel.home/nexus/repository/apod/ch/guengel/apod/apod-crawler/${VERSION}/apod-crawler-${VERSION}-jar-with-dependencies.jar

FROM --platform=$BUILDPLATFORM ghcr.io/graalvm/graalvm-ce:java11 AS runnable
ARG BUILDPLATFORM
ARG USERID=10000
ARG USERNAME=apod-crawler
RUN useradd -U -m -u $USERID $USERNAME &&\
    install -d -o $USERID -g $USERID /download

COPY --from=jar /tmp/*.jar /home/$USERNAME/apod-crawler.jar

WORKDIR /home/$USERNAME
USER $USERID
VOLUME /download
ENTRYPOINT ["/usr/bin/java", "-jar", "apod-crawler.jar", "/download"]
CMD ["--help"]

