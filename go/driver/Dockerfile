FROM amd64/alpine:3.8 as build
RUN echo http://nl.alpinelinux.org/alpine/edge/testing >> /etc/apk/repositories
RUN apk update && apk add build-base go git libzmq zeromq-dev alpine-sdk libsodium-dev
RUN apk add 'go>=1.11-r0' --update-cache --repository http://nl.alpinelinux.org/alpine/edge/community

COPY . .
RUN addgroup -S databox && adduser -S -g databox databox
RUN GGO_ENABLED=0 GOOS=linux go build -a -ldflags '-s -w' -o driver /src/*.go

FROM amd64/alpine:3.8
COPY --from=build /etc/passwd /etc/passwd
RUN apk update && apk add libzmq
USER databox
WORKDIR /
COPY --from=build /driver .
COPY --from=build /static /static
LABEL databox.type="driver"
EXPOSE 8080
CMD ["./driver"]
