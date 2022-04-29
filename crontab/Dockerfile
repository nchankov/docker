FROM alpine:3.6

RUN apk add --no-cache curl

# copy crontabs for root user
COPY config/cronjobs /etc/crontabs/root

RUN chmod 0744 /etc/crontabs/root

# start crond with log level 8 in foreground, output to stderr
CMD ["crond", "-f", "-d", "8"]
