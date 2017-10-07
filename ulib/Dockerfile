FROM gcc:6

WORKDIR /ulib

ENV ULIB_VERSION 1.4.2
ENV ULIB_ROOT /ulib/compiled
ENV ULIB_DOCUMENT_ROOT=${ULIB_ROOT}/ULIB_DOCUMENT_ROOT

# Get ulib
RUN curl -L -o /tmp/ULib-${ULIB_VERSION}.tar.gz https://github.com/stefanocasazza/ULib/archive/v${ULIB_VERSION}.tar.gz \
    && tar -x -C . -f /tmp/ULib-${ULIB_VERSION}.tar.gz

# build it
RUN cd ULib-${ULIB_VERSION} \
&& ./configure --prefix=${ULIB_ROOT} --disable-static --disable-examples \
--without-ssl --without-pcre --without-expat \
--without-libz --without-libuuid --without-magic --without-libares \
--enable-static-server-plugin=http \
    && make install \
    && cd examples/userver \
    && make install

# Copy the sources
COPY ulib/src ./src

# Compile usp pages
RUN mkdir -p ${ULIB_DOCUMENT_ROOT} \
    && cd ULib-${ULIB_VERSION}/src/ulib/net/server/plugin/usp \
    && make json.la plaintext.la \
    && cp .libs/json.so .libs/plaintext.so ${ULIB_DOCUMENT_ROOT}

## Set the number of threads
ENV MAX_THREADS 12

## Configure the default benchmark.cfg
RUN echo 'userver {\n\
    PORT 8080\n\
    PREFORK_CHILD ${MAX_THREADS}\n\
    TCP_LINGER_SET 0\n\ 
    LISTEN_BACKLOG 256\n\
    CLIENT_FOR_PARALLELIZATION 100\n\
    DOCUMENT_ROOT $ULIB_DOCUMENT_ROOT\n \
}' >> $ULIB_ROOT/benchmark.cfg

# 2. Start ULib Server (userver_tcp)
ENV UMEMPOOL "58,0,0,41,273,-15,-14,-20,36"

ENTRYPOINT ["./compiled/bin/userver_tcp","-c", "./compiled/benchmark.cfg"]