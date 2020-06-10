BootStrap: docker
From: centos:centos7

%labels
  Maintainer Manuel Holtgrewe

%help
  This container runs MaxQuant in CentOS 7.

%files
  MaxQuant_1.6.14.zip /tmp

%apprun vncserver
  exec vncserver "${@}"

%apprun vncpasswd
  exec vncpasswd "${@}"

%apprun websockify
  exec /opt/websockify/run "${@}"

%apprun maxquant
  exec mono /opt/MaxQuant/bin/MaxQuantCmd.exe "${@}"

%environment
  export PATH=/opt/TurboVNC/bin:${PATH}

%post
  # Software versions
  export TURBOVNC_VERSION=2.2.1
  export WEBSOCKIFY_VERSION=0.8.0

  # Get dependencies
  yum update -y && yum upgrade -y
  yum install -y epel-release
  yum groupinstall -y "X Window System"
  #yum groupinstall -y "MATE Desktop"
  yum install -y \
    less \
    wget \
    vim \
    unzip

  rpmkeys --import "http://pool.sks-keyservers.net/pks/lookup?op=get&search=0x3fa7e0328081bff6a14da29aa6a19b38d3d831ef"
  curl https://download.mono-project.com/repo/centos7-stable.repo | tee /etc/yum.repos.d/mono-centos7-stable.repo
  yum install -y \
    mono-devel

  mkdir -p /opt
  cd /opt
  unzip /tmp/MaxQuant_1.6.14.zip

  dbus-uuidgen > /etc/machine-id

  # Install TurboVNC https://sourceforge.net/projects/turbovnc/files/2.2.1/turbovnc-2.2.1.x86_64.rpm/download
  wget https://sourceforge.net/projects/turbovnc/files/${TURBOVNC_VERSION}/turbovnc-${TURBOVNC_VERSION}.x86_64.rpm -q
  yum install -y turbovnc-${TURBOVNC_VERSION}.x86_64.rpm
  rm -rf turbovnc-${TURBOVNC_VERSION}.x86_64.rpm

  # Install websockify
  mkdir -p /opt/websockify
  wget https://github.com/novnc/websockify/archive/v${WEBSOCKIFY_VERSION}.tar.gz -q -O - | tar xzf - -C /opt/websockify --strip-components=1
  rm -rf v*.tar.gz
