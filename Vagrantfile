# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
echo "Stopping the network manager"
echo "============================"
if rpm -q NetworkManager; then
  service NetworkManager stop
  yum remove -y NetworkManager
  chkconfig network on
  service network start
fi

echo "Installing epel, git, vim, and pip."
echo "==================================="
sudo yum check-update
sudo yum install -y epel-release git vim
sudo yum install -y python-pip
sudo pip install --upgrade pip git-review

echo "Configuring git"
echo "==============="
su vagrant -c 'whoami'
su vagrant -c 'git config --global user.name "Chris Hoge"'
su vagrant -c 'git config --global user.email "chris@openstack.org"'
su vagrant -c 'git config --global --add gitreview.username "hogepodge"'
su vagrant -c 'git config --list'
su vagrant -c 'cp /vagrant/.ssh/id_rsa* ~/.ssh/.'

echo "Installing Docker"
echo "================="
curl -fsSL https://get.docker.com/ | sh
sudo usermod -aG docker vagrant
sudo systemctl start docker
sudo systemctl enable docker

SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.vm.provision "shell", inline: $script

  config.vm.network "private_network", netmask: "255.255.255.0", ip: "192.168.11.21"
  config.vm.network "private_network", netmask: "255.255.255.0", ip: "192.168.22.21", 
      
  config.vm.provider :vmware_fusion do |vm|
        vm.memory = 1024
        vm.cpus = 1
  end
end
