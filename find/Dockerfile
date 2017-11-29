ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

# Install requirements for add-on
RUN apk add --no-cache ca-certificates jq git musl-dev go tini supervisor mosquitto-clients

# Install python3 stuff for random forest support
RUN apk add --no-cache python3 python3-dev && \
    python3 -m ensurepip && \
    rm -r /usr/lib/python*/ensurepip && \
    pip3 install --upgrade pip setuptools && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    if [[ ! -e /usr/bin/python ]]; then ln -sf /usr/bin/python3 /usr/bin/python; fi && \
    pip install --no-cache-dir numpy scipy scikit-learn && \
    rm -r /root/.cache

# Install FIND
RUN mkdir /usr/local/work
ENV GOPATH /usr/local/work
RUN go get github.com/schollz/find
WORKDIR "/usr/local/work/src/github.com/schollz/find"
RUN go build -v
RUN ln -s /usr/local/work/src/github.com/schollz/find/find /usr/local/bin/find-server

# Copy supervisor config
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

#Set entry point for tini
ENTRYPOINT ["/sbin/tini -s", "--"]

CMD [ "/usr/bin/supervisord" ]
