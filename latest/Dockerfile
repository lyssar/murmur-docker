FROM alpine:3.23.0

ARG BUILD_DATE
ARG VCS_REF
ARG VERSION

LABEL \
    org.label-schema.vendor="cef - devmaint@cefurox.de" \
    org.label-schema.url="https://github.com/cefuroX/murmur-docker" \
    org.label-schema.name="Mumble Server" \
    org.label-schema.version=$VERSION \
    org.label-schema.vcs-url="https://github.com/cefuroX/murmur-docker.git" \
    org.label-schema.vcs-ref=$VCS_REF \
    org.label-schema.build-date=$BUILD_DATE \
    org.label-schema.license="Apache-2.0" \
    org.label-schema.schema-version="1.0.0"

WORKDIR /opt/murmur

COPY ./scripts/setpw /opt/murmur/setpw
COPY ./scripts/setup /opt/murmur/setup

RUN set -x \
    && adduser -DHs /sbin/nologin murmur \
    && apk add -q --no-cache --virtual .build-deps \
        curl \
    && apk add -q --no-cache bash shadow tzdata \
    && apk add -q --no-cache qt5-qtbase-mysql murmur netcat-openbsd \
    && bash -c 'mkdir -p {cert,config,data,log}' \
    && chown -R murmur:murmur /opt/murmur \
    && chmod ug+x /opt/murmur/setpw  /opt/murmur/setup \
    && apk del -q --purge .build-deps \
    && rm -rf /tmp/* /var/tmp/* /var/cache/apk/*

EXPOSE 64738/tcp 64738/udp

USER murmur

VOLUME ["/opt/murmur/cert","/opt/murmur/config","/opt/murmur/data","/opt/murmur/log"]

CMD ./setup && murmurd -fg -ini /opt/murmur/config/murmur.ini