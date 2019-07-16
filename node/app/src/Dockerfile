FROM arm64v8/alpine:3.8


RUN addgroup -S databox && adduser -S -g databox databox && \
apk --no-cache add build-base pkgconfig nodejs npm libzmq zeromq-dev libsodium-dev python sudo && \
cd /home/databox && sudo -u databox npm install zeromq@4.6.0 --zmq-external && \
apk del build-base pkgconfig libsodium-dev python zeromq-dev sudo

USER databox
WORKDIR /home/databox
ADD ./package.json package.json

RUN npm install --production
ADD ./main.js main.js
ADD ./views views

LABEL databox.type="app"

EXPOSE 8080

CMD ["npm","run","start-prod"]
