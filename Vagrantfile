# -*- mode: ruby -*-
# vi: set ft=ruby :

$apt = <<SCRIPT
export DEBIAN_FRONTEND=noninteractive 

# refresh
apt-get update -y

# essentials
apt-get install -y tmux git wget build-essential vim

# requirements.txt deps
apt-get install -y libcurl4-openssl-dev python3-dev libssl-dev # pycurl
apt-get install -y ldap-utils # ldapsearch
apt-get install -y libmariadbclient-dev # mysqlclient-python
apt-get install -y ike-scan unzip default-jdk
apt-get install -y libsqlite3-dev libsqlcipher-dev # pysqlcipher
apt-get install -y libpq-dev # psycopg2

# xfreerdp (see https://github.com/FreeRDP/FreeRDP/wiki/Compilation)
apt-get install ninja-build build-essential git-core debhelper cdbs dpkg-dev autotools-dev cmake pkg-config xmlto libssl-dev docbook-xsl xsltproc libxkbfile-dev libx11-dev libwayland-dev libxrandr-dev libxi-dev libxrender-dev libxext-dev libxinerama-dev libxfixes-dev libxcursor-dev libxv-dev libxdamage-dev libxtst-dev libcups2-dev libpcsclite-dev libasound2-dev libpulse-dev libjpeg-dev libgsm1-dev libusb-1.0-0-dev libudev-dev libdbus-glib-1-dev uuid-dev libxml2-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libfaad-dev libfaac-dev
apt-get install libavutil-dev libavcodec-dev libavresample-dev
git clone https://github.com/FreeRDP/FreeRDP/ /tmp/FreeRDP && (cd /tmp/FreeRDP && cmake -DCMAKE_BUILD_TYPE=Debug -DWITH_SSE2=ON . && make && sudo make install)

SCRIPT

$patator = <<SCRIPT
python3 -m venv patatorenv --without-pip
source patatorenv/bin/activate
wget --quiet -O - https://bootstrap.pypa.io/get-pip.py | python3
pip install patator

SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false
 
  # prevent TTY error messages
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  config.vm.provision "shell",
                      inline: $apt,
                      preserve_order: true,
                      privileged: true 

  config.vm.provision "shell",
                      inline: $patator,
                      preserve_order: true,
                      privileged: false
end
