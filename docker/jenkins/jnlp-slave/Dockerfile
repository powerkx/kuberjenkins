#Base os image
FROM centos:7
MAINTAINER David <davidwang@qifun.com>

# Setup Centos7
USER root
RUN mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
RUN curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo

RUN mkdir -p /app/install/
RUN yum install -y gcc* libxml2 libxml2-devel libxslt libxslt-devel wget vim rpm git crond redhat-lsb epel-release tcl telnet openssl openssl-devel

#Install Dev Tools
#Oracle JDK 8u161
USER root
ADD jdk-8u161-linux-x64.rpm /app/install/
RUN rpm -ivh /app/install/jdk-8u161-linux-x64.rpm

#Svn 1.8.17
ADD wandisco-svn.repo /etc/yum.repos.d/
RUN yum -y install subversion

#Python 3.6.4
ADD Python-3.6.4.tgz /app/install/
RUN cd /app/install/Python-3.6.4 \
  && ./configure \
  && make \
  && make altinstall

COPY jenkins-slave /usr/local/bin/jenkins-slave
RUN chmod +x /usr/local/bin/jenkins-slave

# Setup jenkins slave
ENV HOME /home/jenkins
RUN groupadd -g 10000 jenkins
RUN useradd -c "Jenkins user" -d $HOME -u 10000 -g 10000 -m jenkins
LABEL Description="This is a base image, which provides the Jenkins agent executable (slave.jar)" Vendor="Jenkins project" Version="3.16"

ARG VERSION=3.16
ARG AGENT_WORKDIR=/home/jenkins/agent

RUN curl --create-dirs -sSLo /usr/share/jenkins/slave.jar https://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/${VERSION}/remoting-${VERSION}.jar \
  && chmod 755 /usr/share/jenkins \
  && chmod 644 /usr/share/jenkins/slave.jar

USER jenkins
ENV AGENT_WORKDIR=${AGENT_WORKDIR}
RUN mkdir /home/jenkins/.jenkins && mkdir -p ${AGENT_WORKDIR}

VOLUME /home/jenkins/.jenkins
VOLUME ${AGENT_WORKDIR}
WORKDIR /home/jenkins

ENTRYPOINT ["jenkins-slave"]
