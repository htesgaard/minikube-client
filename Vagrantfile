# -*- mode: ruby -*-
# vi: set ft=ruby :

module OS
  def OS.windows?
    (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
  end

  def OS.mac?
    (/darwin/ =~ RUBY_PLATFORM) != nil
  end

  def OS.unix?
    !OS.windows?
  end

  def OS.linux?
    OS.unix? and not OS.mac?
  end
end

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  #config.vm.box = "base"
  config.vm.box = "centos/7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = true

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  #config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  if (defined?(OS.windows)).nil?
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox",  :mount_options => [ "dmode=775", "fmode=774" ]
  else
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox",  :mount_options => [ "dmode=775", "fmode=774" ], :nfs => true
  end

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  #config.trigger.before [:up, :provision] do
  #  minikube start
  #end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL


  user_home_environment_variable = 'HOME'


  if (defined?(OS.windows)).nil?
    user_home_environment_variable = user_home_environment_variable
  end

  config.vm.provision "file", source: ENV[user_home_environment_variable]+"/.kube/config", destination: "/home/vagrant/.kube/config"
  config.vm.provision "file", source: ENV[user_home_environment_variable]+"/.minikube/ca.crt", destination: "/home/vagrant/.minikube/ca.crt"
  config.vm.provision "file", source: ENV[user_home_environment_variable]+"/.minikube/apiserver.crt", destination: "/home/vagrant/.minikube/apiserver.crt"
  config.vm.provision "file", source: ENV[user_home_environment_variable]+"/.minikube/apiserver.key", destination: "/home/vagrant/.minikube/apiserver.key"

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
if [ ! -f /usr/local/bin/kubectl ]; then
    echo downloading kubectl
    # curl fails when running behind corporate proxy servers, so curl is replaced with wget here
    # curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    sudo yum -y install wget
    wget -c -T 3 https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl -O kubectl
    chmod +x ./kubectl
    echo "move kubectl to /usr/local/bin/kubectl"
    sudo mv ./kubectl /usr/local/bin/kubectl
fi

if [ ! -x "$(which nano)" ]; then
  echo "installing nano"
  sudo yum -y install nano
fi

if [ ! -x "$(which git)" ]; then
  echo "installing git"
  sudo yum -y install git
fi

echo "configure kubectl"
/usr/local/bin/kubectl config set-cluster minikube --certificate-authority=.minikube/ca.crt
/usr/local/bin/kubectl config set-credentials minikube --certificate-authority=.minikube/ca.crt --client-key=.minikube/apiserver.key --client-certificate=.minikube/apiserver.crt

if ! grep -q -i 'kubectl' ~/.bashrc; then
  echo "adding kubectl bash completion to the profile of: $USER"
  echo "source <(kubectl completion bash)" >> ~/.bashrc
fi

echo "Now run 'vagrant ssh' and start some kubectl'ing"

SHELL




end
