# Gogs for OpenShift
Gogs is the Go Git service. Learn more about it at https://gogs.io/

Running containers on OpenShift comes with certain security and other
requirements. This repository contains:

* A Dockerfile for building an OpenShift-compatible Gogs image
* Various scripts used in the Docker image
* OpenShift templates for deploying the image
* Usage instructions

## Deployment
There are two templates available: _persistent_ and _non-persistent_. The pesistent one requires two `PersistentVolume` available with default size required of 1Gi (the volume size can be specified with the template variables: `GOGS_VOLUME_CAPACITY` and `DB_VOLUME_CAPACITY`).

* gogs-data 
* gogs-postgres-data

Both templates will provision two linked PODs: one for GOGS and other for Postgresql DB. If your have persistent volumes available in your cluster:

```
oc new-app -f http://bit.ly/openshift-gogs-persistent-template --param=HOSTNAME=gogs-demo.yourdomain.com
```

Otherwise:
```
oc new-app -f http://bit.ly/openshift-gogs-template --param=HOSTNAME=gogs-demo.yourdomain.com
```
## Accessing GOGs interface
You can access and manage your GOGs app creating an admin user as described in GOGs' official FAQ [1]:

> Administration
> 
> How can I become an administrator?
> 
> The first registered user with ID = 1 is an administrator. No e-mail confirmation is required for this (if enabled). > The default administrator can log into Admin > Users and authorize another user. A user will also be an > administrator if they register in the install page.

[1] https://gogs.io/docs/intro/faqs

## Notes
 * that hostname is required during Gogs installation in order to configure repository urls correctly.

You can deploy any of the available Gogs versions on [openshiftdemos/gogs](https://hub.docker.com/r/openshiftdemos/gogs/tags/) on Docker Hub using the ```GOGS_VERSION``` template parameter:
```
oc new-app -f http://bit.ly/openshift-gogs-template --param=HOSTNAME=gogs-demo.yourdomain.com --param=GOGS_VERSION=0.11.4
```

# Admin User
After Gogs deployment, the first registered user will be admin. The default administrator can log into Admin > Users and authorize another user. A user will also be an > administrator if they register in the install page. Read more on [Gogs FAQ](https://gogs.io/docs/intro/faqs#how-can-i-become-an-administrator%3F)