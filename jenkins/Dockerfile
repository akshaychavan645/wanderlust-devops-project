FROM jenkins/jenkins:lts-jdk21

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends docker-cli docker-buildx sudo \
    && printf '%s\n' 'jenkins ALL=(root) NOPASSWD: /usr/bin/docker' > /etc/sudoers.d/jenkins-docker \
    && chmod 0440 /etc/sudoers.d/jenkins-docker \
    && visudo -cf /etc/sudoers.d/jenkins-docker \
    && rm -rf /var/lib/apt/lists/*

USER jenkins
