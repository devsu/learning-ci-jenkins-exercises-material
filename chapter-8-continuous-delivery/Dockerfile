# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.
FROM ubuntu:bionic
LABEL maintainer="caguirre@devsu.com"

# In case you need proxy
#RUN echo 'Acquire::http::Proxy "http://127.0.0.1:8080";' >> /etc/apt/apt.conf

# Set user jenkins to the image and
# Set hardcode password to user Jenkins (only for demo purposes)
# Insert user jenkins into sudores group
RUN useradd -m -d /home/jenkins -s /bin/sh jenkins &&\
    echo "jenkins\njenkins" | passwd jenkins &&\
    usermod -aG sudo jenkins

# Add locales after locale-gen as needed
# Upgrade packages on image
# Install sshd
# Install JRE 8 (latest edition)
# Install JDK 8 (latest edition)
# Install WGET (latest edition)
# Install CURL (latest edition)
RUN apt-get -q update &&\
    apt-get install -y locales &&\
    locale-gen en_US.UTF-8 &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends &&\
    DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends \
        openssh-server \
        software-properties-common \
        openjdk-8-jre-headless \
        openjdk-8-jdk \
        wget \
        curl &&\
    apt-get -q autoremove &&\
    apt-get -q clean -y && rm -rf /var/lib/apt/lists/* && rm -f /var/cache/apt/*.bin &&\
    sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd &&\
    mkdir -p /var/run/sshd

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Install TOMCAT
# Install JMETER
RUN cd /tmp &&\
    wget \
        https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.39/bin/apache-tomcat-8.5.39.tar.gz \
        https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-5.1.1.tgz &&\
    mkdir \
        /home/jenkins/tomcat \
        /opt/jmeter &&\
    tar xzvf apache-tomcat-8.5.39.tar.gz -C /home/jenkins/tomcat --strip-components=1 &&\
    tar xzvf apache-jmeter-5.1.1.tgz -C /opt/jmeter --strip-components=1 &&\
    chown -R jenkins \
         /home/jenkins/tomcat \
        /opt/jmeter/ &&\
    chmod -R 777 /opt/jmeter/

# Standard SSH port
EXPOSE 22

# Default command
CMD ["/usr/sbin/sshd", "-D"]