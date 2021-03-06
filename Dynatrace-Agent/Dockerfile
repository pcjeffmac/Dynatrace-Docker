FROM debian:wheezy

MAINTAINER Martin Etmajer <martin.etmajer@dynatrace.com>

ENV VERSION                 "6.5"
ENV NODE_INSTALLER64_URL    "https://dl.dropboxusercontent.com/u/61124235/dynatrace-one-agent-nodejs-6.5.0.1278-linux-x86.tgz"
ENV WSAGENT_INSTALLER32_URL "https://dl.dropboxusercontent.com/u/61124235/dynatrace-wsagent-6.5.0.1278-linux-x86-32.tar"
ENV WSAGENT_INSTALLER64_URL "https://dl.dropboxusercontent.com/u/61124235/dynatrace-wsagent-6.5.0.1278-linux-x86-64.tar"

ENV DT                      "/dynatrace"
ENV AGENT_LIB32             "${DT}/agent/lib/libdtagent.so"
ENV AGENT_LIB64             "${DT}/agent/lib64/libdtagent.so"
ENV NODE_AGENT              "${DT}/agent/nodejs/nodejsagent.js"
ENV WSAGENT_BIN64           "${DT}/agent/lib64/dtwsagent"
ENV WSAGENT_INI             "${DT}/agent/conf/dtwsagent.ini"
ENV SLAVE_AGENT_PORT        "8001"

ENV  DT_INSTALL_DEPS "curl"
ENV  DT_RUNTIME_DEPS "procps"
COPY build/scripts/install-node-agent.sh /usr/bin
COPY build/scripts/install-wsagent.sh /usr/bin
RUN  apt-get update && \
     apt-get install -y --no-install-recommends ${DT_INSTALL_DEPS} ${DT_RUNTIME_DEPS} && \
     mkdir -p ${DT} && \
     install-wsagent.sh ${WSAGENT_INSTALLER32_URL} && \
     install-wsagent.sh ${WSAGENT_INSTALLER64_URL} && \
     install-node-agent.sh ${NODE_INSTALLER64_URL} && \
     apt-get remove --purge -y ${DT_INSTALL_DEPS} && \
     rm -rf /var/lib/apt/lists/*
ADD  build/bin/dtnginx_offsets.json.tar.gz ${DT}/agent/conf
COPY build/scripts/run-wsagent.sh ${DT}

CMD while true; do sleep 1; done