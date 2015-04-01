Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_url = "https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/14.04/providers/virtualbox.box"
  config.vm.provision :shell, :inline => $BOOTSTRAP_SCRIPT # see below
end

$BOOTSTRAP_SCRIPT = <<EOF
  set -e # Stop on any error

  export DEBIAN_FRONTEND=noninteractive

  echo VAGRANT SETUP: DOING APT-GET STUFF...
  # Install prereqs
  sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
  echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
  sudo apt-get update

  sudo apt-get install -y python-software-properties make git-core curl g++ htop redis-server=2:2.8.4-2 mongodb-org=2.6.9

  echo VAGRANT SETUP: DOING NODE STUFF...
  export NODE_VERSION=0.12
  # Install Node with nvm
  sudo -u vagrant git clone git://github.com/creationix/nvm.git ~vagrant/nvm
  echo "source ~vagrant/nvm/nvm.sh; nvm use $NODE_VERSION" | tee -a ~vagrant/.profile
  source ~vagrant/nvm/nvm.sh
  nvm install v$NODE_VERSION
  nvm use $NODE_VERSION

  # Make vagrant automatically go to /vagrant when we ssh in.
  echo "cd /vagrant" | sudo tee -a ~vagrant/.profile


  echo VAGRANT IS READY.
EOF
