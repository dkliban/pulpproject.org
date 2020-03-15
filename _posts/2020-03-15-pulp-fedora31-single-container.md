---
title: Pulp 3 in One Container
author: Dennis Kliban
tags:
  - 3.0
---
Installing Pulp 3 and getting all the the services running can be challenging. In an effort to make
these steps optional, we have been working on creating a single container image that has all the
services needed to run Pulp 3. The initial version of this image is now available under my namespac
Dockerhub. This image includes Ansible, Container, File, Maven, and RPM plugins. The following
commands can be used to start up Pulp 3.2.0:

$ mkdir settings pulp_storage pgsql
$ echo "CONTENT_ORIGIN='$(hostname):8080'" >> settings/settings.py

$ podman run -v ./settings:/etc/pulp \
             -v ./pulp_storage:/var/lib/pulp \
             -v ./pgsql:/var/lib/pgsql \
              --publish 8080:80 \
              --detach \
              dkliban/pulp-fedora31
38fe951a6f169653dca86259f2480dc47b0b1fa77875290d44ccbad3b9e15a70            

$ podman exec -it 38fe951a6f169653dca86259f2480dc47b0b1fa77875290d44ccbad3b9e15a70 /bin/bash
# pulpcore-manager reset-admin-password
Please enter new password for user "admin": 
Please enter new password for user "admin" again: 
Successfully set password for "admin" user.
