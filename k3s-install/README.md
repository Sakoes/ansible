k3s-install playbook for rpi
===============================
## Overview
This playbook can be run on a fresh raspbian install. It downloads, configures k3s and runs all tools and applications I like to work with.  
To be able to run this playbook, the community.kubernetes plugin is needed. It is defined in the [requirements.yml](https://github.com/Sakoes/ansible/blob/main/k3s-install/roles/requirements.yml) file.

## Roles
Each role is tagged with the name of the role. By specifying this tag when exectuting the playbook, the specific role can be executed without the other roles, thus making it possible to only install certain services/applications. 

### install-config
The install-config makes some configurations on the rpi to be able to run k3s. After these configurations, it downloads and install k3s. 

### resources
This roles adds some custome resources to the k3s installation:
- Installs Helm package manager
- Adds helm repos
- Installs Metallb and configures it as cluster loadbalancer
- Installs nginx reverse-proxy to use as ingress/ingress-controller

### dashboard
This role downloads and installs a kubernetes dashboard and a dashboard-metrics-scraper. It also creates an admin user and generates an access token in orde to connect to the dashboard.
The role uses two files:
- [k8s-dashboard.yml](https://github.com/Sakoes/ansible/blob/main/k3s-install/roles/dashboard/files/k8s-dashboard.yml) which defines all necessary resources for the dashboard and the metrics scraper like deployments, services, secrets, roles, serviceaccount...
- [servcice-account.yml](https://github.com/Sakoes/ansible/blob/main/k3s-install/roles/dashboard/files/service-account.yml) with the user-admin service account

### certificate-authority
>cert-manager adds certificates and certificate issuers as resource types in Kubernetes clusters, and simplifies the process of obtaining, renewing and using those certificates.  

This role configures installs a certificate authority _cert-manager_. It generates a private key, creates a certificate with this key and encodes both in base64 to adds them to a _nginx-ca-secret.yml_. It creates the secret in the cluster and als creates a ClusterIssuer with this secret.  
By running this CA, other applications (nextcloud, jenkins...) can be run locally with https without having to expose the service to external CA's or the internet.

### nextcloud
>Nextcloud puts your data at your fingertips, under your control. Store your documents, calendar, contacts and photos on a server at home. 

This role creates/install all resources to be able to run nextcloud. All resources are defined in [nextcloud/files](https://github.com/Sakoes/ansible/blob/main/k3s-install/roles/nextcloud/files):
- nextcloud namespace
- PV & PVC
- Helm repo & Helm chart values
- Certificate
- Ingress

### jenkins
>Jenkins – an open source automation server which enables developers around the world to reliably build, test, and deploy their software.  

This role creates/install all resources to be able to run a jenkins server. All resources are defined in [jenkins/files](https://github.com/Sakoes/ansible/blob/main/k3s-install/roles/jenkins/files)::
- jenkins namespace
- PV & PVC
- Helm repo & Helm chart values
- Service account
- Ingress

### media-center
__This role is not finished, the applications are not being used. Is a work in progress__  
This role runs Jackett torrent client, sonarr as a torrent indexer and plex as a media server.
## How to use
```
ansible-galaxy install -r roles/requirements.yml 
```
```
ansible-playbook k3s-install.yml 
```
OR 
```
ansible-playbook k3s-install.yml --tags nextcloud 
```
