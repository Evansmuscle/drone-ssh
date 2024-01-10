FROM alpine:3.17

ARG TARGETOS
ARG TARGETARCH

LABEL maintainer="Kaan Korkmaz <katokorkmaz@gmail.com>" \
  org.label-schema.name="SSH Plugin" \
  org.label-schema.vendor="Kaan Korkmaz" \
  org.label-schema.schema-version="1.0"

LABEL org.opencontainers.image.source=https://github.com/Evansmuscle/drone-ssh
LABEL org.opencontainers.image.description="Execute commands on a remote host through SSH"
LABEL org.opencontainers.image.licenses=MIT

RUN apk add --no-cache ca-certificates && \
  rm -rf /var/cache/apk/*

RUN addgroup \
  -S -g 1000 \
  deploy && \
  adduser \
  -S -H -D \
  -h /home/deploy \
  -s /bin/sh \
  -u 1000 \
  -G deploy \
  deploy

RUN mkdir -p /home/deploy && \
  chown deploy:deploy /home/deploy
  
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION v20.10.0

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# deploy:deploy
USER 1000:1000

COPY release/${TARGETOS}/${TARGETARCH}/drone-ssh /bin/

ENTRYPOINT ["/bin/drone-ssh"]
