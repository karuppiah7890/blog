---
title: "Installing Prometheus Operator, Prometheus, Alertmanager and Grafana"
date: 2020-03-26T19:31:37+05:30
---

In this blog post I'm going to teach about how to install
[Prometheus Operator](https://github.com/coreos/prometheus-operator),
[Prometheus](https://github.com/prometheus/prometheus),
[Alertmanager](https://github.com/prometheus/alertmanager) and
[Grafana](https://github.com/grafana/grafana)

If you want to checkout the video version of this blog which shows a demo, you
can see the below video

{{<youtube agpRm3sKBlo>}}

This blog post has written in response to a message here - 
https://lists.cncf.io/g/cncf-helm/message/293 . Usually I don't like to give
away answers easily, but sometimes it feels like - why do things have to be
hard? Why does tech have to be hard? Also, I know that it takes quite sometime
to understand some nuances in dev ops and find all the information from all
the scattered places and understand what's going on. This is not a complete
attempt to teach everything. This is more like a simple manual guide on how to
install things without explaining too much details!

Install the latest release of Helm from your package manager based on your OS.
In my case, I manually downloaded the tar ball from the
[releases](https://github.com/helm/helm/releases) page and used it for
installation. Since I use Mac OS 64 bit, I got the appropriate `darwin` 64 bit
tar ball.

The latest version of Helm as of this writing is `v3.1.2`

```bash
$ wget https://get.helm.sh/helm-v3.1.2-darwin-amd64.tar.gz
$ tar xf helm-v3.1.2-darwin-amd64.tar.gz
$ mv darwin-amd64/helm /usr/local/bin/helm
$ # cleanup
$ rm -rf helm-v3.1.2-darwin-amd64.tar.gz darwin-amd64/
```

Before we install prometheus operator or anything, we need a kubernetes cluster
to install it in. Since I don't have any cloud based kubernetes cluster, I'm
going to run a local kubernete cluser using `minikube`. You can install
`minikube` by using the binary from their [releases](https://github.com/kubernetes/minikube/releases)
page. I installed it using (`gofish`)[https://gofi.sh]

```bash
$ gofish install minikube
```

Note: You can also install `helm` too using `gofish` :) I install multiple
versions (v2 and v3) of helm in my local and manage them, so I don't use
`gofish` to install helm

Now let's start our kubernetes cluster using `minikube`!

```bash
$ minikube start --kubernetes-version=v1.18.0
😄  minikube v1.8.2 on Darwin 10.15.3
✨  Automatically selected the hyperkit driver. Other choices: docker, virtualbox
🔥  Creating hyperkit VM (CPUs=2, Memory=4000MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.18.0 on Docker 19.03.6 ...
    > kubelet.sha256: 65 B / 65 B [--------------------------] 100.00% ? p/s 0s
    > kubectl.sha256: 65 B / 65 B [--------------------------] 100.00% ? p/s 0s
    > kubeadm.sha256: 65 B / 65 B [--------------------------] 100.00% ? p/s 0s
    > kubeadm: 37.96 MiB / 37.96 MiB [---------------] 100.00% 3.47 MiB p/s 12s
    > kubelet: 108.01 MiB / 108.01 MiB [-------------] 100.00% 6.92 MiB p/s 16s
    > kubectl: 41.98 MiB / 41.98 MiB [---------------] 100.00% 2.31 MiB p/s 19s
🚀  Launching Kubernetes ...
🌟  Enabling addons: default-storageclass, storage-provisioner
⌛  Waiting for cluster to come online ...
🏄  Done! kubectl is now configured to use "minikube"
```

Check if the kubernetes cluster is up and running

```bash
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.3", GitCommit:"06ad960bfd03b39c8310aaf92d1e7c12ce618213", GitTreeState:"clean", BuildDate:"2020-02-11T18:14:22Z", GoVersion:"go1.13.6", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.0", GitCommit:"9e991415386e4cf155a24b1da15becaa390438d8", GitTreeState:"clean", BuildDate:"2020-03-25T14:50:46Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"}

$ kubectl get pods -n kube-system
NAME                          READY   STATUS    RESTARTS   AGE
coredns-66bff467f8-24swn      1/1     Running   0          15m
coredns-66bff467f8-d5dwg      1/1     Running   0          15m
etcd-m01                      1/1     Running   0          15m
kube-apiserver-m01            1/1     Running   0          15m
kube-controller-manager-m01   1/1     Running   0          15m
kube-proxy-df68x              1/1     Running   0          15m
kube-scheduler-m01            1/1     Running   0          15m
storage-provisioner           1/1     Running   1          15m
```

Now we can search for charts to install. As of this writing, `stable` chart
repository still exists along with
[`incubator`](https://github.com/helm/charts/#how-do-i-enable-the-incubator-repository).
You can find the source code here - https://github.com/helm/charts/ . But in the
future it will not exist, instead charts will be maintained in a more
distributed manner and we can use [Helm Hub](https://hub.helm.sh/) to search for
charts across public repositories that have listed themselves in
the Hub. And charts will also be present in OCI registries soon, with Helm3
providing that feature. I'll provide both the ways to search for charts - in
locally added repositories including stable, and to search in Helm Hub. And then
how to install them too!

For searching / checking in chart repositories

```bash
$ # check if you have stable repository
$ # or any other repository in which you want to search in
$ helm repo list
Error: no repositories to show
$ # or else add it
$ helm repo add stable https://kubernetes-charts.storage.googleapis.com/
"stable" has been added to your repositories
$ helm search repo prometheus-operator
NAME                            CHART VERSION   APP VERSION     DESCRIPTION
stable/prometheus-operator      8.12.3          0.37.0          Provides easy monitoring definitions for Kubern...
```

For searching in Helm Hub

```bash
$ helm search hub prometheus-operator
URL                                                     CHART VERSION   APP VERSION     DESCRIPTION
https://hub.helm.sh/charts/choerodon/prometheus...      8.5.8           8.5.8           Provides easy monitoring definitions for Kubern...
https://hub.helm.sh/charts/cloudposse/prometheu...      0.2.0                           Provides easy monitoring definitions for Kubern...
https://hub.helm.sh/charts/cloudposse/prometheus        0.2.1                           Prometheus instance created by the CoreOS Prome...
https://hub.helm.sh/charts/stable/prometheus-op...      8.12.3          0.37.0          Provides easy monitoring definitions for Kubern...
https://hub.helm.sh/charts/bitnami/prometheus-o...      0.13.8          0.38.0          The Prometheus Operator for Kubernetes provides...
```

Now, I'm going to install `prometheus-operator` chart from `stable` chart
repository - https://hub.helm.sh/charts/stable/prometheus-operator

```bash
$ helm install prom-operator stable/prometheus-operator
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
NAME: prom-operator
LAST DEPLOYED: Thu Mar 26 20:27:10 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
The Prometheus Operator has been installed. Check its status by running:
  kubectl --namespace default get pods -l "release=prom-operator"

Visit https://github.com/coreos/prometheus-operator for instructions on how
to create & configure Alertmanager and Prometheus instances using the Operator.
```

Ignore the below info message

```bash
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
```

I think this is due to the fact that the helm chart is trying to support both v2
and v3, and hence the above info. `crd-install` is a concept in v2, not in v3
though.

Looks like our helm chart has been installed. Let's check all the resources!

```bash
$ helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                      APP VERSION
prom-operator   default         1               2020-03-26 20:27:10.579598 +0530 IST    deployed        prometheus-operator-8.12.3 0.37.0

$ kubectl get pod
NAME                                                     READY   STATUS    RESTARTS   AGE
alertmanager-prom-operator-prometheus-o-alertmanager-0   2/2     Running   0          106s
prom-operator-grafana-7f86956496-xj7v4                   3/3     Running   0          2m7s
prom-operator-kube-state-metrics-6dcd4c8fbd-jtzfp        1/1     Running   0          2m7s
prom-operator-prometheus-node-exporter-tjgz7             1/1     Running   0          2m7s
prom-operator-prometheus-o-operator-64964d9fb7-mw467     2/2     Running   0          2m7s
prometheus-prom-operator-prometheus-o-prometheus-0       3/3     Running   1          96s
```

```bash
$ kubectl get crd
NAME                                    CREATED AT
alertmanagers.monitoring.coreos.com     2020-03-26T14:57:05Z
podmonitors.monitoring.coreos.com       2020-03-26T14:57:05Z
prometheuses.monitoring.coreos.com      2020-03-26T14:57:05Z
prometheusrules.monitoring.coreos.com   2020-03-26T14:57:06Z
servicemonitors.monitoring.coreos.com   2020-03-26T14:57:06Z
thanosrulers.monitoring.coreos.com      2020-03-26T14:57:06Z
```

As you can see, on installing the `prometheus-operator` chart, I have installed
the prometheus operator, prometheus, alertmanager and grafana too! 

Let's checkout the dashboard for each of them! Starting with Prometheus!

```bash
$ kubectl port-forward --namespace='default' prometheus-prom-operator-prometheus-o-prometheus-0 9090
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
Handling connection for 9090
```

Now visit http://localhost:9090 !

Next, Alertmanager!

```bash
$ kubectl port-forward --namespace='default' alertmanager-prom-operator-prometheus-o-alertmanager-0 9093
Forwarding from 127.0.0.1:9093 -> 9093
Forwarding from [::1]:9093 -> 9093
Handling connection for 9093
```

Now visit http://localhost:9093 !

Next up is Grafana!

```bash
$ kubectl port-forward --namespace='default' prom-operator-grafana-7f86956496-xj7v4 3000
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
Handling connection for 3000
```

Now visit http://localhost:3000 ! The default admin username is `admin` and
default admin password is `prom-operator`

You can visit some dashboards and check if they are working. For me some of
them worked, some didn't. Like, `Nodes` dashboard worked, but `etcd` didn't.

So, that's how you install Prometheus Operator, Prometheus, Alertmanager and
Grafana

If you have any questions, please comment below or you can find me on the
[kubernetes slack](https://kubernetes.slack.com) @karuppiah7890 ! :) You can
join the kubernetes slack by using this auto invitation
app - https://slack.kubernetes.io

