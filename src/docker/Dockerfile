ARG REGISTRY=docker.io
FROM ${REGISTRY}/alpine:latest
LABEL maintainer="dev_lau11@a.jlv6.com"
LABEL os="alpine"
RUN apk --update -t add --no-cache haproxy curl libcap iproute2 && \
    rm -f /var/cache/apk/* /tmp/* && \
    rm -f /sbin/halt /sbin/poweroff /sbin/reboot
COPY ./haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg
COPY ./sni-backend-443.map /usr/local/etc/haproxy/sni-backend-443.map
RUN <<EOF
  addgroup -S haproxy && adduser -S -s /sbin/nologin -G haproxy haproxy
  chown haproxy:haproxy /usr/local/etc/haproxy/haproxy.cfg
  chmod 774 /usr/local/etc/haproxy/haproxy.cfg
  chown haproxy:haproxy /usr/local/etc/haproxy/sni-backend-443.map
  chmod 774 /usr/local/etc/haproxy/sni-backend-443.map
  chown haproxy:haproxy /usr/sbin/haproxy
  chmod 4774 /usr/sbin/haproxy
  setcap 'cap_net_bind_service=+ep' /usr/sbin/haproxy
EOF
USER haproxy
# Start HAProxy (-db = do not background)
CMD ["/usr/sbin/haproxy", "-f", "/usr/local/etc/haproxy/haproxy.cfg", "-db"]
