FROM nodered/node-red:latest-18

USER root

RUN mkdir -p /config && chown node-red:node-red /config && chmod -R 777 /config \
    && mkdir -p /db && chown node-red:node-red /db && chmod -R 777 /db \
    && mkdir -p /static && chown node-red:node-red /static && chmod -R 777 /static \
    && mkdir -p /tests && chown node-red:node-red /tests && chmod -R 777 /tests

USER node-red

RUN npm i \
    @flowfuse/node-red-dashboard \
    @flowfuse/node-red-dashboard-2-ui-led \
    fast-deep-equal
#    @wdns/vue-code-block
