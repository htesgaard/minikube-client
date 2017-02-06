# minikube-client

Project that allows users on Windows, running minikube, to
interact with minikube using kubectl running on Linux.

This project spawns a Virtualbox guest Linux OS using Vagrant, copies the necessary files from userprofile folders
.kube and .minikube folders into the similar location on the Guest OS.

This allows allowing interaction with the same minikube cluster using kubectl from both Windows and a Linux.

##Prerequisites

###Required
1. Minikube configured and running
1. Windows (Tested on Windows 8.1)
1. Virtualbox (+VM VirtualBox Extension Pack)
1. Vagrant
1. Vagrant plugins


```cmd
C:\>vagrant plugin list
vagrant-share (1.1.6, system)
vagrant-vbguest (0.13.0)
```

Install missing plugins using the command `vagrant plugin install <name>` 

> The plugin `vagrant-vbguest` automatically installs the virtualbox guest additions, on `vagrant up`, that's necessary
for file sharing between the guests and host to work.

###Optional
1. Cygwin




