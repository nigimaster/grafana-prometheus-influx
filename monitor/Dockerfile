FROM centos

EXPOSE 3000 9090 8083 8086

WORKDIR /opt
ADD yum.repo /etc/yum.repos.d/

# UPDATE
RUN yum update -y && \
    yum install -y epel-release && \
    yum install -y  python3-setuptools && \
    yum -y install python3-pip && \
    yum clean all
RUN yum -y install git influxdb grafana
# RUN easy_install supervisor
RUN pip3 install supervisor
RUN curl -L -o prometheus.tar.gz https://github.com/prometheus/prometheus/releases/download/v2.29.2/prometheus-2.29.2.linux-amd64.tar.gz
RUN mkdir prometheus
RUN tar zxf prometheus.tar.gz --strip-components=1 -C prometheus
RUN git clone https://github.com/percona/grafana-dashboards.git
ADD grafana.ini /etc/grafana/
ADD supervisord.conf /etc/
ADD prometheus.yml /opt/prometheus/

VOLUME ["/var/lib/grafana", "/opt/prometheus/data", "/var/lib/influxdb"]

CMD ["supervisord", "-c", "/etc/supervisord.conf"]
