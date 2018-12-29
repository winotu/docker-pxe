FROM fedora:29

RUN dnf update -y && dnf clean all
RUN dnf install -y dhcp-server /usr/sbin/ip && dnf clean all

EXPOSE 67/udp
RUN setcap CAP_NET_BIND_SERVICE,CAP_NET_RAW=+ep /usr/sbin/dhcpd
RUN chmod o+rx /etc/dhcp/

COPY --chown=dhcpd dhcpd.conf /etc/dhcp/dhcpd.conf

USER dhcpd

ENTRYPOINT ["/usr/sbin/dhcpd"]
CMD ["-d", "--no-pid"]
