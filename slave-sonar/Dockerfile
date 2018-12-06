FROM openshift/origin

MAINTAINER Ben Parees <bparees@redhat.com>

ENV HOME=/home/jenkins

USER root
# Install headless Java
RUN yum install -y centos-release-scl-rh && \
    x86_EXTRA_RPMS=$(if [ "$(uname -m)" == "x86_64" ]; then echo -n java-1.8.0-openjdk-headless.i686 ; fi) && \
    INSTALL_PKGS="bc gettext git java-1.8.0-openjdk-headless lsof rsync tar unzip which zip bzip2" && \
    yum install -y --setopt=tsflags=nodocs install $INSTALL_PKGS $x86_EXTRA_RPMS && \
    # have temporarily removed the validation for java to work around known problem fixed in fedora; jupierce and gmontero are working with
    # the requisit folks to get that addressed ... will switch back to rpm -V $INSTALL_PKGS when that occurs
    rpm -V bc gettext git lsof rsync tar unzip which zip bzip2  && \
    yum clean all && \
    rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm && \
    yum -y install puppet-agent && \
    mkdir -p /home/jenkins && \
    chown -R 1001:0 /home/jenkins && \
    chmod -R g+w /home/jenkins && \
    chmod 664 /etc/passwd && \
    chmod -R 775 /etc/alternatives && \
    chmod -R 775 /var/lib/alternatives && \
    chmod -R 775 /usr/lib/jvm && \
    chmod 775 /usr/bin && \
    chmod 775 /usr/lib/jvm-exports && \
    chmod 775 /usr/share/man/man1 && \
    chmod 775 /var/lib/origin
COPY sonar-scanner-3.2.0.1227-linux /opt/sonar
COPY hosts /etc/hosts
ENV sonarqubeurl sonarqube-homolog.badesul.com.br 
ENV PATH "$PATH:/opt/sonar/"
# Copy the entrypoint
ADD contrib/bin/* /usr/local/bin/
RUN /usr/local/bin/run-jnlp-client
# Run the Jenkins JNLP client
RUN /opt/puppetlabs/bin/puppet
RUN /bin/bash
ENTRYPOINT /bin/sh
#ENTRYPOINT /opt/sonar/bin/sonar-scanner -Dsonar.login=2ee7db26ee4d6fd9e89c9d1a6395cdc7e81238fc
