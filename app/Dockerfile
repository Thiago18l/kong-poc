FROM registry.access.redhat.com/ubi8/ubi

RUN yum update -y
RUN yum install hostname -y
RUN curl -Lo /tmp/kong.rpm $(rpm --eval "https://download.konghq.com/gateway-3.x-rhel-%{rhel}/Packages/k/kong-3.2.2.rhel%{rhel}.amd64.rpm")
RUN yum install /tmp/kong.rpm -y \
    && rm -f /tmp/kong.rpm \
    && rm -rf /var/cache/yum \
    && chown kong:0 /usr/local/bin/kong \
    && chown -R kong:0 /usr/local/kong \
    && ln -s /usr/local/openresty/luajit/bin/luajit /usr/local/bin/luajit \
    && ln -s /usr/local/openresty/luajit/bin/luajit /usr/local/bin/lua \
    && ln -s /usr/local/openresty/nginx/sbin/nginx /usr/local/bin/nginx \
    && kong version

RUN yum install curl -y \
    && curl -sL https://github.com/kong/deck/releases/download/v1.19.1/deck_1.19.1_linux_amd64.tar.gz -o /tmp/deck.tar.gz \
    && tar -xf /tmp/deck.tar.gz -C /tmp \
    && cp /tmp/deck /usr/local/bin/ \
    && rm -rf /var/cache/yum \
    && rm -rf /tmp/*

ADD docker-entrypoint.sh /usr/local/bin/
ADD waitport /usr/local/bin
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
RUN chmod +x /usr/local/bin/waitport
RUN mkdir /kong \
    && chown kong:0 /kong

USER kong

VOLUME [ "/kong" ]

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]

CMD [ "kong", "docker-start" ]

WORKDIR /kong

EXPOSE 8000 8443 8001 8444 8005 8006