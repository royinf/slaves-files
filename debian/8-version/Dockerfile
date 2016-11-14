FROM debian:latest

# Install sudo, SSH and compilers
RUN apt-get -q update
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q upgrade -y -o Dpkg::Options::="--force-confnew" --no-install-recommends
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends sudo ca-certificates openssh-server
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends git build-essential gcc make libssl-dev clang check valgrind libsubunit0 libsubunit-dev
RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends python

RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends geoip-bin geoip-database libgeoip-dev libgeoip1 libpq-dev libpq5 libmysqlclient-dev

RUN DEBIAN_FRONTEND="noninteractive" apt-get -q install -y -o Dpkg::Options::="--force-confnew" --no-install-recommends aptitude
RUN aptitude  -y install mingw-w64 nsis

RUN apt-get -q clean -y && rm -rf /var/lib/apt/lists/* && rm -f /var/cache/apt/*.bin
RUN mkdir /var/run/sshd

# Set user to image
RUN useradd -m -d /home/jenkins -s /bin/sh jenkins && echo "jenkins:MYPASSWORD" | chpasswd && adduser jenkins sudo
RUN echo 'jenkins ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers.d/00-jenkins-user-file

# Standard SSH port
EXPOSE 22

# Default command
CMD ["/usr/sbin/sshd", "-D"]

