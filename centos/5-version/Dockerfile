FROM centos:5

# Install sudo, SSH and compilers
RUN yum -y update
RUN yum install -y sudo ca-certificates openssh-server
RUN yum groupinstall -y 'Development Tools'


RUN yum install -y gcc make openssl-devel clang valgrind epel-release wget tar
RUN yum install -y libsubunit subunit subunit-devel check

RUN yum -y update

RUN yum install -y git

RUN yum install -y GeoIP GeoIP-data GeoIP-devel postgresql-libs postgresql-devel mysql mysql-devel mysql-lib



RUN yum install -y python
RUN mkdir /var/run/sshd
RUN ssh-keygen -b 2048 -t rsa -C"$(id -un)@$(hostname)-$(date --rfc-3339=date)" -f /etc/ssh/ssh_host_rsa_key -N ""


# Set user to image
RUN adduser -m -d /home/jenkins -s /bin/sh jenkins && echo "jenkins:MYPASSWORD" | chpasswd && usermod -aG wheel jenkins
RUN echo 'jenkins ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN echo 'Defaults:jenkins !requiretty' >> /etc/sudoers
RUN echo 'Defaults:root !requiretty' >> /etc/sudoers

# Standard SSH port
EXPOSE 22

# Default command
CMD ["/usr/sbin/sshd", "-D"]

