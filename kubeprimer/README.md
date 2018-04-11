# KubePrimer Workshop

Welcome to the KubePrimer workshop repository. This workshop is organized by [SIGHUP](http://sighup.io), the aim is to introduce all the participants to the basics concepts behind [Kubernetes](http://kubernetes.io).

# Preparation & Setup

You will need to have a laptop (Windows, Linux or Mac), it is preferred for your machine to have at least 8GB of RAM. This isn't strictly required but highly recommended. Working with VMs and Containers **is** resource intensive, no way to get around it.

This workshop requires that you have the following installed on your machine:
- [Virtualbox](https://www.virtualbox.org/)
- [Minikube](https://github.com/kubernetes/minikube#installation)

In order to speed up the setup process, we also recommend you to download the VMs & containers required during the workshop.

### Setting up Kubectl
Kubectl is the CLI controller app for kubernetes. You will use to interact with all the kubernetes cluster. Therefore it is required to be able to perform *any* task during the workshop.

[Here how to install it](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Setting up minikube

Minikube is a simple one-node installation of Kubernetes specifically designed to work on your local machine.

Do the following commands to setup the minikube installation:

1. `minikube start`, this will setup the minikube VM
2. `minikube ssh` to get into the machine
3. From inside the machine do `docker pull sighup/kubeprimer-web`, `docker pull sighup/kubeprimer-backend`, `docker pull mongo:3.7.3`
4. Stop minikube without destroying it with `minikube stop`

If you don't know what those commands are for or how they work specifically, do not worry. We will review them during the workshop.

## Accounts

Make also sure to have a github account properly configured.

