from jenkins/jenkins:2.89.3

#FROM jenkins/jenkins:lts
#COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
#RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# install Build Tools plugins
# RUN /usr/local/bin/install-plugins.sh ant:1.9.10
# RUN /usr/local/bin/install-plugins.sh gradle:4.5.1

# install Python3
USER root
RUN apt-get update \
  && apt-get install -y python3-pip python3-dev \
  && cd /usr/local/bin \
  && ln -s /usr/bin/python3 python \
  && pip3 install --upgrade pip


