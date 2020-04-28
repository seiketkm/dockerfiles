FROM ubuntu:18.04 as build-env
RUN apt-get update && apt-get -y install wget build-essential cmake zlib1g-dev &&\
    apt-get clean &&\
    rm -rf /var/lib/apt/lists/*
ARG VERSION=4.20
ARG FILENAME=knp-${VERSION}.tar.bz2 
ARG TARGET_DIR=/var/task
RUN wget http://nlp.ist.i.kyoto-u.ac.jp/nl-resource/knp/${FILENAME}
RUN tar -xf ${FILENAME} &&\
    cd knp-${VERSION} &&\
    ./configure --prefix=${TARGET_DIR}
RUN cp /knp-${VERSION}/system/make_cf_index.c /knp-${VERSION}/system/make_cf_index # 
RUN cd /knp-${VERSION} && make && make install

FROM seiketkm/jumanpp:v2-rc3 as jumanpp-env

FROM ubuntu:18.04 as run-env
ARG TARGET_DIR=/var/task
COPY --from=jumanpp-env ${TARGET_DIR}/bin/jumanpp \
    ${TARGET_DIR}/bin/jumanpp
COPY --from=jumanpp-env ${TARGET_DIR}/libexec/jumanpp/jumandic.config \
    ${TARGET_DIR}/libexec/jumanpp/jumandic.config
COPY --from=jumanpp-env ${TARGET_DIR}/libexec/jumanpp/jumandic.jppmdl \
    ${TARGET_DIR}/libexec/jumanpp/jumandic.jppmdl
COPY --from=build-env ${TARGET_DIR}/bin/knp \
    ${TARGET_DIR}/bin/knp
COPY --from=build-env ${TARGET_DIR}/etc/knprc \
    ${TARGET_DIR}/etc/knprc
COPY --from=build-env ${TARGET_DIR}/share/juman/dic \
    ${TARGET_DIR}/share/juman/dic
COPY --from=build-env ${TARGET_DIR}/share/knp/rule \
    ${TARGET_DIR}/share/knp/rule
COPY --from=build-env ${TARGET_DIR}/share/knp/dict \
    ${TARGET_DIR}/share/knp/dict
COPY --from=build-env ${TARGET_DIR}/lib \
    ${TARGET_DIR}/lib
RUN ln -s ${TARGET_DIR}/bin/jumanpp /bin/jumanpp && \
    ln -s ${TARGET_DIR}/bin/knp /bin/knp
ENTRYPOINT ["sh", "-c", "/bin/jumanpp | /bin/knp"]
