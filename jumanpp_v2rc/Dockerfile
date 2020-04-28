FROM ubuntu:18.04 as build-env
ARG RC=rc3
ARG FILE_NAME=jumanpp-2.0.0-${RC}.tar.xz
ARG JUMANPP_DOWNLOADLINK=https://github.com/ku-nlp/jumanpp/releases/download/v2.0.0-${RC}/${FILE_NAME}
ARG TARGET_DIR=/var/task
RUN apt-get update && apt-get -y install wget build-essential cmake zlib1g-dev &&\
    apt-get clean &&\
    rm -rf /var/lib/apt/lists/*
RUN wget ${JUMANPP_DOWNLOADLINK} &&\
    tar -xf ${FILE_NAME}
RUN mkdir -p ${TARGET_DIR} &&\
    cd ${TARGET_DIR} &&\
    cmake /jumanpp-2.0.0-${RC} \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=${TARGET_DIR} &&\
    make install -j4

FROM ubuntu:18.04 as run-env
ARG TARGET_DIR=/var/task
COPY --from=build-env ${TARGET_DIR}/bin/jumanpp \
    ${TARGET_DIR}/bin/jumanpp
COPY --from=build-env ${TARGET_DIR}/libexec/jumanpp/jumandic.config \
    ${TARGET_DIR}/libexec/jumanpp/jumandic.config
COPY --from=build-env ${TARGET_DIR}/libexec/jumanpp/jumandic.jppmdl \
    ${TARGET_DIR}/libexec/jumanpp/jumandic.jppmdl
RUN ln -s ${TARGET_DIR}/bin/jumanpp /bin/jumanpp 
ENTRYPOINT ["/bin/jumanpp"]
