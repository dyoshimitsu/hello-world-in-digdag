FROM amazoncorretto:8

RUN apk add --no-cache curl && \
    curl -o ~/bin/digdag --create-dirs -L "https://dl.digdag.io/digdag-latest"
    chmod +x ~/bin/digdag
    adduser -h /var/lib/digdag -g 'digdag user' -s /sbin/nologin -D digdag && \
    mkdir -p /var/lib/digdag/logs/tasks /var/lib/digdag/logs/server && \
    chown -R digdag.digdag /var/lib/digdag && \
    rm -rf /var/cache/apk/*

USER digdag
WORKDIR /var/lib/digdag

ENV DB_TYPE=postgresql \
    DB_USER=digdag \
    DB_PASSWORD=digdag \
    DB_HOST=postgresql \
    DB_PORT=5432 \
    DB_NAME=digdag

EXPOSE 65432
CMD exec digdag server --bind $SERVER_BIND \
                       --port $SERVER_PORT \
                       --config /etc/server.properties \
                       --params-file /var/lib/digdag/config.yml \
                       --log /var/lib/digdag/logs/server \
                       --task-log /var/lib/digdag/logs/tasks \
                       -X database.type=$DB_TYPE \
                       -X database.user=$DB_USER \
                       -X database.password=$DB_PASSWORD \
                       -X database.host=$DB_HOST \
                       -X database.port=$DB_PORT \
                       -X database.database=$DB_NAME \
                       -X database.maximumPoolSize=32
