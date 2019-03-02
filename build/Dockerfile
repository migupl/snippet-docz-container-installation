FROM alpine:3.9

ENV DOCZ_HOST 0.0.0.0

RUN apk update && \
    apk add --no-cache yarn && \
    rm -rf /var/cache/apk/*

EXPOSE 3000

WORKDIR /doc_your_things

USER documentator

CMD ["yarn", "docz:dev"]
