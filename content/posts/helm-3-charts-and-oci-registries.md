---
title: "Helm 3 Charts and OCI registries"
date: 2019-06-18T22:00:00+05:30
---

Recently Helm 3 alpha version was released https://github.com/helm/helm/releases/tag/v3.0.0-alpha.1 🎉. There are lot of new things in this release, I wanted to write about one of these things - Pushing Charts to OCI Registries!

With helm 3, you can push your charts to container registries similar to pushing container images! TLDR; Show me your terminal!

{{<asciinema 252441>}}


The commands being used in the video:


```
$ docker run -dp 5000:5000 --restart=always --name registry registry
$ helm create my-chart
$ ls
$ helm chart save my-chart localhost:5000/my-chart:0.1.0
$ helm chart list
$ helm chart push localhost:5000/my-chart:0.1.0
$ rm -rf my-chart
$ helm chart rm localhost:5000/my-chart:0.1.0
$ helm chart list
$ helm chart pull localhost:5000/my-chart:0.1.0
$ helm chart list
$ helm chart export localhost:5000/my-chart:0.1.0
$ ls
```

I tried pushing charts to Docker Hub, but it currently doesn't work. You can check this issue - https://github.com/helm/helm/issues/5861

Resources you can check to read more upon this:

Docs about Helm 3 Registries: https://github.com/helm/helm/blob/dev-v3/docs/registries.md

Helm 3 support in preview in Azure Container Registrt: https://azure.microsoft.com/en-us/updates/azure-container-registry-helm-3-support-is-now-in-preview/

OCI spec repo: https://github.com/opencontainers/distribution-spec/