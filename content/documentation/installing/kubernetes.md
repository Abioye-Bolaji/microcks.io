---
draft: false
title: "Installing on Kubernetes"
date: 2019-09-01
publishdate: 2019-09-01
lastmod: 2020-10-12
menu:
  docs:
    parent: installing
    name: Installing on Kubernetes
    weight: 10
toc: true
weight: 10 #rem
---

## Instructions

One easy way if installing Microcks is to do it on Kubernetes. Kubernetes in version 1.6 or greater is required. It is assumed that you have some kind of Kubernetes cluster up and running available. This can take several forms depending on your environment and needs:

* Lightweight Minikube on your laptop, see [Minikube project page](https://github.com/kubernetes/minikube),
* Google Cloud Engine account in the cloud, see how to start a [Free trial](https://console.cloud.google.com/freetrial),
* Any other Kubernetes distribution provider.

We provide a <code>Chart</code> for using with [Helm](https://helm.sh/) Packet Manager.

## Helm 3

Helm 3 chart has been tested from Kubernetes 1.17 and is now available on our own repository. This allow to install Microcks with just 3 commands:

```sh
$ helm repo add microcks https://microcks.io/helm

$ kubectl create namespace microcks

$ helm install microcks microcks/microcks —-version 1.0.0 --namespace microcks --set microcks.url=microcks.$(minikube ip).nip.io --set keycloak.url=keycloak.$(minikube ip).nip.io
```

After some minutes and components have been deployed, you should end up with a Spring-boot Pod, a MongoDB Pod, a Postman-runtime Pod, a Keycloak Pod and a PostgreSQL Pod like in the screenshot below.

<img src="/images/running-pods-k8s.png" class="img-responsive"/>

Now you can retrieve the URL of the created ingress using <code>kubectl get ingress -n microcks</code>. Before starting playing with Microcks, you'll have to connect to Keycloak component in order to configure an identity provider or define some users for the Microcks realm (see [Keycloak documentation](http://www.keycloak.org/docs/latest/server_admin/index.html#user-management)). Connection to Keycloak can be done using username and password stored into a <code>microcks-keycloak-config</code> secret created during setup.

For full instructions and deployment optinos, we recommand reading the [README](https://github.com/microcks/microcks/blob/master/install/kubernetes/README.md) on GitHub repository.

## Minikube configuration

When using Minikube, depending on what you've already deployed, default configuration may be little bit slow and induce timeout on readiness probes. We usually setup the following configuration for successfull deployments:

```sh
minikube config set cpus 4
minikube config set memory 6144
minikube delete
minikube start
```