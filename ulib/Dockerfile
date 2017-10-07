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


## Configure the default benchmark.cfg
RUN echo 'userver {\n\
    PORT 8080\n\
    PREFORK_CHILD 2\n\
    TCP_LINGER_SET 0\n\ 
    LISTEN_BACKLOG 16384\n\
    CLIENT_FOR_PARALLELIZATION 100\n\
    DOCUMENT_ROOT $ULIB_DOCUMENT_ROOT\n \
}' >> $ULIB_ROOT/benchmark.cfg

# Copy the sources
COPY src ./src

# Compile usp pages
RUN mkdir -p ${ULIB_DOCUMENT_ROOT} \
    && cd ULib-${ULIB_VERSION}/src/ulib/net/server/plugin/usp \
    && make json.la plaintext.la \
    && cp .libs/json.so .libs/plaintext.so ${ULIB_DOCUMENT_ROOT}
