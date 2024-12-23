---
title: "Running eBPF Tools in Minikube"
linkTitle: "Running eBPF Tools in Minikube"
weight: 1
date: 2019-08-15
description: >
  Running eBPF Tools in Minikube
---

## Overview

eBPF tools are performance tools used for observing the Linux kernel.
These tools can be used to monitor your Kubernetes application in minikube.
This tutorial will cover how to set up your minikube cluster so that you can run eBPF tools from a Docker container within minikube.


## Requirement
- use VM Driver (not docker or podman)
- x86 (currently the [bcc image](https://hub.docker.com/r/zlim/bcc/tags) does not support arm64)
- Latest minikube version

## Tutorial

First, start minikube with a VM driver:

```
$ minikube start --vm=true
```


You can now run [BCC tools](https://github.com/iovisor/bcc) as a Docker container in minikube:

```shell
$ minikube ssh -- docker run --rm   --privileged   -v /lib/modules:/lib/modules:ro   -v /usr/src:/usr/src:ro   -v /etc/localtime:/etc/localtime:ro   --workdir /usr/share/bcc/tools   zlim/bcc ./execsnoop


Unable to find image 'zlim/bcc:latest' locally
latest: Pulling from zlim/bcc
6cf436f81810: Pull complete
987088a85b96: Pull complete
b4624b3efe06: Pull complete
d42beb8ded59: Pull complete
90970d1ebfd9: Pull complete
29c3815350eb: Pull complete
e21dfbd8fcfc: Pull complete
Digest: sha256:914bea8970535cd6b0d5dee13f99569c5f0d597942c8333c0aa92443473aff27
Status: Downloaded newer image for zlim/bcc:latest
PCOMM            PID    PPID   RET ARGS
runc             5059   2011     0 /usr/bin/runc --version
docker-init      5065   2011     0 /usr/bin/docker-init --version
nice             5066   4012     0 /usr/bin/nice -n 19 du -x -s -B 1 /var/lib/kubelet/pods/1cf22976-f3e0-498b-bc04-8c7068e6e545/volumes/kubernetes.io~secret/storage-provisioner-token-cvk4x
```

