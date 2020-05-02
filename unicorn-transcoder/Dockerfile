FROM alpine:3.9
MAINTAINER donicrosby

ARG GLIBC_VERSION="2.29-r0"
ARG S6_OVERLAY_VERSION="1.22.1.0"
ARG PLEX_UID="1000"
ARG PLEX_GID="1000"

ENV \
  PLEX_ARCH="amd64" \
  UNICORN_PREFIX="/opt/UnicornTranscoder" \
  PLEX_UID=${PLEX_UID} \
  PLED_GID=${PLEX_GID}

RUN \
  apk add --no-cache npm git curl libxml2-utils binutils file shadow && \
  curl -J -L -o /tmp/glibc-${GLIBC_VERSION}.apk https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk && \
  curl -J -L https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz | tar zxvf - -C / && \
  apk add --allow-untrusted /tmp/glibc-${GLIBC_VERSION}.apk && \
  ln -s /lib/libz.so.1 /usr/glibc-compat/lib/libz.so.1 && \
  ln -s /lib/libc.musl-x86_64.so.1 /usr/glibc-compat/lib/libc.musl-x86_64.so.1 && \
  addgroup --system --gid=${PLEX_GID} plex && \
  adduser --system --uid=${PLEX_UID} --ingroup=plex plex && \
  git -C /opt clone https://github.com/UnicornTranscoder/UnicornTranscoder.git && \
  npm config set cache ${UNICORN_PREFIX}/.npm && \
  npm install --prefix=${UNICORN_PREFIX} && \
  apk del git && \
  rm -rf /var/cache/apk/* /tmp/*

COPY rootfs/ /

EXPOSE 3000/tcp

ENTRYPOINT ["/init"]
