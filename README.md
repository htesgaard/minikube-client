# minikube-client

This project  allows users on Windows, running minikube, to
interact with minikube using kubectl running on virtualized Linux.

This project spawns a Virtualbox guest Linux OS using Vagrant, copies the necessary kubectl and minikube files from the
Host OS userprofile folder, into the similar location on the Guest OS.

This allows allowing interaction with the same minikube cluster, using kubectl, from both Windows and a Linux.

##Prerequisites

###Required prerequisites
1. Minikube configured and running
```cmd 
C:\>minikube status
minikubeVM: Running
localkube: Running
```
2. Some OS running Vagrant (Tested on Windows 8.1 and macOS Sierra)
3. VirtualBox (optional: VirtualBox Extension Pack)
4. Vagrant
5. Vagrant plugins installed
```cmd
C:\>vagrant plugin list
vagrant-share (1.1.6, system)
vagrant-vbguest (0.13.0)
```

Install missing plugins using the command `vagrant plugin install <name>` 

> The plugin `vagrant-vbguest` automatically installs the virtualbox guest additions, on `vagrant up`, that's necessary
for file sharing between the guests and host to work.

###Optional prerequisites
1. Cygwin

#Usage
When minikube is running, at the current directory being at the same location as `Vagrantfile`, run `vagrant up` and then `vagrant ssh`

If kubectl can't find the cluster it is much likely because your ip-address has changed. In that case run `vagrant provision` and then `vagrant ssh`

## Examples

Congratulations! You're now ready to use your Kubernetes cluster, from a kubectl cmd running on a virtualized Linux.

It's now time to get your hands dirty by trying som of the  
Kubernetes [tutorials] (https://kubernetes.io/docs/tutorials/) here 
and Kubernetes [tasks] (https://kubernetes.io/docs/tasks/)

If you just want to test something simple, start with [Kubernetes examples]
(https://github.com/GoogleCloudPlatform/kubernetes/blob/master/examples/).

For a more elaborate scenario [here]
(https://github.com/pires/kubernetes-elasticsearch-cluster) you'll find all
you need to get a scalable Elasticsearch cluster on top of Kubernetes in no
time.






