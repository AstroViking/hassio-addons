ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

# Install requirements for add-on
RUN apk add --no-cache jq git go mosquitto-clients \
  && mkdir -p /usr/local \
  && cd /usr/local \
  && git clone https://github.com/schollz/find.git find-server \
  && cd find-server \
  && go get ./... \
  && go build \
  && ln -s /usr/local/find-server/find /usr/local/bin/find-server

# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
