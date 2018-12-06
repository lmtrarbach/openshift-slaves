FROM centos:latest
MAINTAINER Stefan Lehmann <stefan.lehmann@unic.com>

# Install a basic SSH server GIT, UNZIP, LSOF and JDK 8
RUN yum install -y openssh-server git unzip lsof java-1.8.0-openjdk-headless && yum clean all
# update sshd settings, create jenkins user, set jenkins user pw, generate ssh keys
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd \
    && mkdir -p /var/run/sshd \
    && useradd -u 1000 -m -s /bin/bash jenkins \
    && echo "jenkins:jenkins" | chpasswd \
    && /usr/bin/ssh-keygen -A \
    && echo export JAVA_HOME="/`alternatives  --display java | grep best | cut -d "/" -f 2-6`" >> /etc/environment
RUN rpm -ivh http://yum.puppetlabs.com/puppetlabs-release-el-7.noarch.rpm
RUN yum -y install puppet
# Set java environment
ENV JAVA_HOME /etc/alternatives/jre

# Standard SSH port
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
