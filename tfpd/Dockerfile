FROM fedora:29

RUN dnf update -y
RUN dnf install -y tftp-server rsyslog && dnf clean all

RUN sed -i -e 's#SysSock.Use="off"#SysSock.Use="on"#' -e '/imjournal/d' /etc/rsyslog.conf

# tworzyc tftp usera i z niego uruchamiac?

EXPOSE 69/udp

#ENTRYPOINT ["/usr/sbin/in.tftpd"]

#CMD ["-vvv", "-L", "-s", "/var/lib/tftpboot"]

# ===================== DEBUG ============================

ENTRYPOINT /usr/sbin/rsyslogd && /usr/sbin/in.tftpd -vvv -L -s /var/lib/tftpboot
