FROM jenkins/jenkins:lts

# Install the custom plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# Create the admin account in jenkins
ENV JENKINS_OPTS \
    --argumentsRealm.passwd.admin=${ADMIN_PASSWORD} \
    --argumentsRealm.roles.user=admin \
    --argumentsRealm.roles.admin=admin

# Install docker, unfortunately it's not in the debian repositories, so we need to add a repository
USER root

# Install some prerequisites
RUN apt-get update && \
     apt-get install -y \
     software-properties-common \
     apt-transport-https \
     ca-certificates \
     curl \
     sudo \
     gnupg2

# Install the repository
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
     add-apt-repository \
     "deb [arch=amd64] https://download.docker.com/linux/debian \
     $(lsb_release -cs) \
     stable"

# Update and install docker
RUN apt-get update && \
     apt-get install -y docker-ce

# Now we need to allow jenkins to run docker commands! (This is not elegant, but at least it's semi-portable...)

## allowing jenkins user to run sudo commands
RUN echo "jenkins ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

## let's make a home directory for jenkins
RUN mkdir /home/jenkins

## avoid typing sudo in command line
RUN echo "alias docker='sudo docker '" >> /home/jenkins/.bashrc
RUN echo "alias docker='sudo docker '" >> $JENKINS_HOME/.bashrc

# switch back to the jenkins-user
USER jenkins