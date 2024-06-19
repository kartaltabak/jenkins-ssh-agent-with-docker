FROM jenkins/ssh-agent:jdk21

RUN echo PATH=$PATH >> /etc/environment

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get -y install apt-utils \
    && apt-get -y install \
    ca-certificates \
    curl \
    gnupg

RUN install -m 0755 -d /etc/apt/keyrings && \
    ( curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg ) && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    ( echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null )

ARG DOCKER_GROUP_ID=1001
RUN groupadd -g ${DOCKER_GROUP_ID} docker

RUN apt-get update \
    && apt-get -y install docker-ce \
    && usermod -aG docker jenkins

ARG JENKINS_WORKSPACE_DIR=/home/jenkins/workspace
RUN mkdir ${JENKINS_WORKSPACE_DIR} && \
    chown -R jenkins:jenkins ${JENKINS_WORKSPACE_DIR}

VOLUME /home/jenkins/workspace

