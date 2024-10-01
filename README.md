# Downloading and running Jenkins in Docker

There are several Docker images of Jenkins available.

Use the recommended official [jenkins/jenkins image](https://hub.docker.com/r/jenkins/jenkins/) from the [Docker Hub repository](https://hub.docker.com/). This image contains the current [Long-Term Support (LTS) release of Jenkins](https://www.jenkins.io/download), which is production-ready.

## [On macOS and Linux](https://www.jenkins.io/doc/book/installing/docker/)

I will use Docker Compose configuration for a quick installation of Jenkins.

1. Customize the official Jenkins Docker image, by executing the following two steps:

- Create a Dockerfile with the following content

```bash
FROM jenkins/jenkins:2.462.2-jdk17
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
```

- Build a new docker image from this Dockerfile, and assign the image a meaningful name I will use Docker Compose

---

## Setup

1. Build a new docker image from this Dockerfile, and assign the image a meaningful name, such as "myjenkins-blueocean"

```bash
docker build -t myjenkins-blueocean .
```

2. Run your Jenkins

```bash
docker compose up -d

# If you wish to stop Jenkins and get back to it later, run:
docker compose down

# Run the following comand to terminate Jenkins and to remove all volumes and images used:
docker compose down --volumes --rmi all
```
