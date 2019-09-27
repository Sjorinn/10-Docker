FROM ubuntu:18.04
EXPOSE 443 80 22
RUN apt-get update -q \
   && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
   ca-certificates openssh-server apt-transport-https tzdata curl
RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-get install gitlab-ce
ARG external_url=https://192.168.99.100/
ENV EXTERNAL_URL=$external_url
ENV GITLAB_ROOT_PASSWORD=aledoskour
RUN echo "external_url" \'$EXTERNAL_URL\' > /etc/gitlab/gitlab.rb
RUN echo "nginx['enable']" = true >> /etc/gitlab/gitlab.rb
RUN echo "nginx['redirect_http_to_https']" = true >> /etc/gitlab/gitlab.rb
RUN echo "nginx['redirect_http_to_https_port']" = 80 >> /etc/gitlab/gitlab.rb
RUN echo "nginx['ssl_certificate']" = '"/etc/gitlab/ssl/gitlab.crt"' >> /etc/gitlab/gitlab.rb
RUN echo "nginx['ssl_certificate_key']" = '"/etc/gitlab/ssl/gitlab.key"' >> /etc/gitlab/gitlab.rb
RUN mkdir -p /etc/gitlab/ssl
RUN openssl genrsa -out "/etc/gitlab/ssl/gitlab.key" 2048
RUN openssl req -new -key "/etc/gitlab/ssl/gitlab.key" -out "/etc/gitlab/ssl/gitlab.csr" -subj "/C=FR/ST=Ile de France/L=Paris/O=42student/OU=hmst/CN=aled.com"
RUN openssl x509 -req -days 365 -in "/etc/gitlab/ssl/gitlab.csr" -signkey "/etc/gitlab/ssl/gitlab.key" -out "/etc/gitlab/ssl/gitlab.crt"
RUN /opt/gitlab/embedded/bin/runsvdir-start & gitlab-ctl reconfigure
ENTRYPOINT service ssh start & /opt/gitlab/embedded/bin/runsvdir-start


# docker build . --build-arg external_url=$(echo https://$(docker-machine ip Char)/) -t ex03
# docker run -p 443:443 -p 80:80 -p 25:22 -d ex03

## Options: git config --global http.sslVerify false
