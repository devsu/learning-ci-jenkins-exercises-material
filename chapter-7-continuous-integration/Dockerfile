# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.
FROM ubuntu:bionic
LABEL maintainer="caguirre@devsu.com"

# In case you need proxy
#RUN echo 'Acquire::http::Proxy "http://127.0.0.1:8080";' >> /etc/apt/apt.conf

# Add locales after locale-gen as needed
# Upgrade packages on image
# Install sshd
# Install JRE 8 (latest edition)
# Install JDK 8 (latest edition)
# Install Maven (latest edition)
# Install GIT (latest edition)
RUN apt-get -q update &&\
    apt-get install -y locales &&\
    locale-gen en_US.UTF-8 &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
        openssh-server \
        software-properties-common \
        openjdk-8-jre-headless \
        openjdk-8-jdk \
        git \
        maven &&\
    apt-get -q autoremove &&\
    apt-get -q clean -y && rm -rf /var/lib/apt/lists/* && rm -f /var/cache/apt/*.bin &&\
    sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd &&\
    mkdir -p /var/run/sshd

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Set user jenkins to the image and
# Set hardcode password to user Jenkins (only for demo purposes)
# Insert user jenkins into sudores group
RUN useradd -m -d /home/jenkins -s /bin/sh jenkins &&\
    echo "jenkins\njenkins" | passwd jenkins &&\
    usermod -aG sudo jenkins

# Standard SSH port
EXPOSE 22

# Default command
CMD ["/usr/sbin/sshd", "-D"]
