FROM centos:6

# Install sudo, SSH and compilers
RUN yum -y update
RUN yum install -y sudo ca-certificates openssh-server
RUN yum groupinstall -y 'Development Tools'
RUN yum install -y git gcc make openssl-devel clang valgrind epel-release wget tar

RUN yum install -y python

RUN yum install -y GeoIP GeoIP-data GeoIP-devel postgresql-libs postgresql-devel mysql mysql-devel mysql-lib

RUN wget --quiet https://github.com/libcheck/check/releases/download/0.10.0/check-0.10.0.tar.gz
RUN tar xzvf check-0.10.0.tar.gz
RUN cd check-0.10.0 && ./configure && make install && cd .. && cp /usr/local/lib/*check* /usr/lib64/


#For install MinGW and other libraries for the Nsis compilation
RUN yum install -y gcc-c++ scons mingw32-gcc mingw64-gcc zlib-devel bzip2 unzip

#For install Nsis
RUN wget --quiet http://downloads.sourceforge.net/project/nsis/NSIS%203%20Pre-release/3.0b2/nsis-3.0b2-src.tar.bz2
RUN wget --quiet http://downloads.sourceforge.net/project/nsis/NSIS%203%20Pre-release/3.0b2/nsis-3.0b2.zip
RUN mkdir /usr/local/nsis
RUN mv nsis-3.0b2-src.tar.bz2 nsis-3.0b2.zip /usr/local/nsis &&  cd /usr/local/nsis  &&  tar -jxvf nsis-3.0b2-src.tar.bz2 && unzip nsis-3.0b2.zip

#Build makensis
RUN cd /usr/local/nsis/nsis-3.0b2-src/ &&  scons SKIPSTUBS=all SKIPPLUGINS=all SKIPUTILS=all SKIPMISC=all NSIS_CONFIG_CONST_DATA=no PREFIX=/usr/local/nsis/nsis-3.0b2 install-compiler
RUN mkdir /usr/local/nsis/nsis-3.0b2/share && cd /usr/local/nsis/nsis-3.0b2/share &&  ln -s /usr/local/nsis/nsis-3.0b2 nsis && cp ../bin/makensis /bin


RUN cp /usr/i686-w64-mingw32/sys-root/mingw/bin/libwinpthread-1.dll /usr/i686-w64-mingw32/lib/


RUN mkdir /var/run/sshd
RUN ssh-keygen -b 2048 -t rsa -C"$(id -un)@$(hostname)-$(date --rfc-3339=date)" -f /etc/ssh/ssh_host_rsa_key -N ""

# Set user to image
RUN adduser -m -d /home/jenkins -s /bin/sh jenkins && echo "jenkins:MYPASSWORD" | chpasswd && usermod -aG wheel jenkins
RUN echo 'jenkins ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers.d/00-jenkins-user-file
RUN echo 'Defaults:jenkins !requiretty' >> /etc/sudoers
RUN echo 'Defaults:root !requiretty' >> /etc/sudoers

# Standard SSH port
EXPOSE 22

# Default command
CMD ["/usr/sbin/sshd", "-D"]

