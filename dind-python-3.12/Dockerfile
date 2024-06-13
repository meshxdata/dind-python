FROM python:3.12-slim

############
## Docker ##
############
ARG DOCKER_CHANNEL=stable

RUN apt-get update && apt-get install -y --no-install-recommends \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg2 \
  software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -

RUN install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc && \
    chmod a+r /etc/apt/keyrings/docker.asc

RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null




RUN apt-get update && apt-get install -y --no-install-recommends docker-ce && \
  docker -v && \
  dockerd -v

####################
## Docker Compose ##
####################
ARG DOCKER_COMPOSE_VERSION=1.29.2
RUN curl -L https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-Linux-x86_64 > /usr/local/bin/docker-compose && \
  chmod +x /usr/local/bin/docker-compose

#################
## DIND Script ##
#################
ARG DIND_COMMIT=deda3d4933d3c0bd57f2cef672da5d28fc653706
ENV DOCKER_EXTRA_OPTS '--storage-driver=overlay'
RUN curl -fL -o /usr/local/bin/dind "https://raw.githubusercontent.com/moby/moby/${DIND_COMMIT}/hack/dind" && \
	chmod +x /usr/local/bin/dind


################
## Entrypoint ##
################
COPY entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh


VOLUME /var/lib/docker
EXPOSE 2375
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
