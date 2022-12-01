FROM ubuntu
RUN apt-get update && apt-get install -y wget gpg nginx openjdk-17-jre openssl

RUN echo "Setup ngnix"
COPY ./default.conf /etc/ngnix/conf.d/default.conf
COPY ./html/index.html /var/www/html/index.html
COPY ./html/500.html /var/www/html/500.html
COPY ./html/404.html /var/www/html/404.html
RUN echo "Generate cert <TODO> Replace with actual cert in live/future"
RUN mkdir -p /opt/ssl
RUN cd /opt/ssl/ && touch supermq.crt && touch supermq.key \
    && openssl req -new -x509 -nodes -days 365 -newkey rsa:2048 -keyout supermq.key -out supermq.crt \
    -subj "/C=IN/ST=gokapp/L=chennai/O=Dis/CN=supermq.org"

RUN echo "Download activemq binaries"
RUN mkdir -p /opt/activemq && cd /opt/activemq \
    && wget https://downloads.apache.org/activemq/5.17.2/apache-activemq-5.17.2-bin.tar.gz \
    && wget https://downloads.apache.org/activemq/5.17.2/apache-activemq-5.17.2-bin.tar.gz.asc \
    && pwd && ls -lrth && echo "Verifying binaries"
RUN cd /opt/activemq && wget https://downloads.apache.org/activemq/KEYS \
    && gpg --import KEYS && gpg --verify apache-activemq-5.17.2-bin.tar.gz.asc apache-activemq-5.17.2-bin.tar.gz
RUN echo "Extracting binaries"
RUN cd /opt/activemq && tar -xzf apache-activemq-5.17.2-bin.tar.gz

RUN apt-get purge openssl -y
RUN apt-get autoremove -y

EXPOSE 443/tcp 80/tcp
CMD ["nginx", "-g", "daemon off;"]
