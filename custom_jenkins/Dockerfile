# Use a Jenkins LTS image based on Debian 12 (Bookworm) for stability
FROM jenkins/jenkins:lts-jdk17

# Switch to root user to install Docker
USER root

# Install Docker Engine using the current Debian Bookworm repo method
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        gnupg \
        lsb-release && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
      https://download.docker.com/linux/debian \
      bookworm stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update -y && \
    apt-get install -y --no-install-recommends \
        docker-ce \
        docker-ce-cli \
        containerd.io \
        docker-buildx-plugin \
        docker-compose-plugin && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Add Jenkins user to the Docker group
RUN groupadd -f docker && usermod -aG docker jenkins

# Prepare Docker volume for DinD
RUN mkdir -p /var/lib/docker
VOLUME /var/lib/docker

# Switch back to Jenkins user
USER jenkins


#HERE IS THE CLI COMMAND TO START THE CONTAINER

#docker run -d   --name jenkins-dind   -p 8080:8080 -p 50000:50000   -v jenkins_home:/var/jenkins_home   -v /var/run/docker.sock:/var/run/docker.sock     jenkins-dind

# EXPLANATION

# -v jenkins_home:/var/jenkins_home → persists Jenkins data.

# -v /var/run/docker.sock:/var/run/docker.sock → lets Jenkins talk to the host’s Docker daemon.

# --group-add $(getent group docker | cut -d: -f3) → gives the Jenkins user permissions to use Docker.

# Ports:

# 8080 → Jenkins web UI

# 50000 → Jenkins agent connections



# INSTALL THESE PACKAGES INTO YOUR DOCKER CONTAINER

# 1. Login container bash terminal

#     docker exec -u root -it jenkins-dind bash
  

# 2. Install Python and Pip:**

#     apt update -y
#     apt install -y python3
#     python3 --version
#     ln -s /usr/bin/python3 /usr/bin/python
#     python --version
#     apt install -y python3-pip
#     apt install -y python3-venv
    