---
title: "debug/buildinfo golang standard library package"
date: 2021-12-15T17:07:00+05:30
draft: true
---

A friend (Madhav) recently posted about the `debug/buildinfo` standard library package in Golang in my company slack channel

https://pkg.go.dev/debug/buildinfo

It is a package present from golang `1.18beta1` version

So I figured I'll give it a try than `defer` it for later ;)

I got the beta version using [`gvm`](https://github.com/moovweb/gvm)

```bash
gvm list-all -u

gvm install 1.18beta1

gvm set 1.18beta1

go version
```

Then I used VS Code editor to start off a small demo project to try out the `debug/buildinfo` package. VS Code has become my default editor for work and especially for Golang. I use the [official Golang VS Code Extension](https://marketplace.visualstudio.com/items?itemName=golang.Go) from the [Go Team at Google](https://marketplace.visualstudio.com/publishers/golang) which is also [Open Source on GitHub](https://github.com/golang/vscode-go/)

I also updated the `gopls` Language Server tool which is used in the Golang VS Code Extension using `Go: Install/Update Tools` VS Code Command and then choosing `gopls` from the list. I did this so that I can get the latest features of the language server and can also get some intellisense features for the latest Golang version `1.18beta1` that I downloaded in my VS Code Editor

The basic demo code is open source here https://github.com/karuppiah7890/temp-trials/tree/master/golang-stuff/buildinfo-demo

I just wrote some simple code to get some build information from a given binary

```go
package main

import (
	"debug/buildinfo"
	"fmt"
	"log"
	"os"
)

func main() {
	if len(os.Args) != 2 {
		log.Fatal("pass exactly one argument")
	}
	binPath := os.Args[1]
	bin, err := os.Open(binPath)
	if err != nil {
		panic(err)
	}

	buildInfo, err := buildinfo.Read(bin)
	if err != nil {
		panic(err)
	}

	fmt.Printf("%+v\n", buildInfo)

	for _, dep := range buildInfo.Deps {
		fmt.Printf("%+v\n", dep)
	}
}
```

and then built it

This simple program could be used with any binary built using `go` tool. I used it with a CLI called `tanzu` CLI which is a golang binary built from the [Tanzu Framework open source project](https://github.com/vmware-tanzu/tanzu-framework/)

This is how the run looked like

```bash
buildinfo-demo $ go build -v

buildinfo-demo $ tanzu version
version: v0.10.0
buildDate: 2021-11-16
sha: fd96beb

buildinfo-demo $ which tanzu
/usr/local/bin/tanzu

buildinfo-demo $ ./buildinfo-demo /usr/local/bin/tanzu
&{GoVersion:go1.16.2 Path:github.com/vmware-tanzu/tanzu-framework/cmd/cli/tanzu Main:{Path:github.com/vmware-tanzu/tanzu-framework Version:(devel) Sum: Replace:<nil>} Deps:[0xc000a540c0 0xc000a54100 0xc000a54140 0xc000a54180 0xc000a541c0 0xc000a54240 0xc000a54280 0xc000a542c0 0xc000a54300 0xc000a54340 0xc000a54380 0xc000a543c0 0xc000a54400 0xc000a54440 0xc000a54480 0xc000a544c0 0xc000a54500 0xc000a54540 0xc000a54580 0xc000a545c0 0xc000a54600 0xc000a54680 0xc000a546c0 0xc000a54700 0xc000a54740 0xc000a54780 0xc000a547c0 0xc000a54800 0xc000a54840 0xc000a548c0 0xc000a54900 0xc000a54940 0xc000a54980 0xc000a549c0 0xc000a54a00 0xc000a54a40 0xc000a54a80 0xc000a54ac0 0xc000a54b00 0xc000a54b40 0xc000a54b80 0xc000a54bc0 0xc000a54c00 0xc000a54c40 0xc000a54c80 0xc000a54cc0 0xc000a54d00 0xc000a54d40 0xc000a54d80 0xc000a54dc0 0xc000a54e00 0xc000a54e40 0xc000a54e80 0xc000a54ec0 0xc000a54f00 0xc000a54f40 0xc000a54f80 0xc000a54fc0 0xc000a55000 0xc000a55040 0xc000a55080 0xc000a550c0 0xc000a55100 0xc000a55140 0xc000a55180 0xc000a551c0 0xc000a55200 0xc000a55240 0xc000a55280 0xc000a552c0 0xc000a55300 0xc000a55340 0xc000a55380 0xc000a553c0 0xc000a55400 0xc000a55440 0xc000a554c0 0xc000a55500 0xc000a55580 0xc000a55600 0xc000a55640 0xc000a556c0 0xc000a55740 0xc000a55780 0xc000a55800] Settings:[]}
&{Path:cloud.google.com/go Version:v0.74.0 Sum:h1:kpgPA77kSSbjSs+fWHkPTxQ6J5Z2Qkruo5jfXEkHxNQ= Replace:<nil>}
&{Path:cloud.google.com/go/storage Version:v1.10.0 Sum:h1:STgFzyU5/8miMl0//zKh2aQeTyeaUH3WN9bSUiJ09bA= Replace:<nil>}
&{Path:github.com/AlecAivazis/survey/v2 Version:v2.1.1 Sum:h1:LEMbHE0pLj75faaVEKClEX1TM4AJmmnOh9eimREzLWI= Replace:<nil>}
&{Path:github.com/Masterminds/semver Version:v1.5.0 Sum:h1:H65muMkzWKEuNDnfl9d70GUjFniHKHRbFPGBuZ3QEww= Replace:<nil>}
&{Path:github.com/adrg/xdg Version:v0.2.1 Sum:h1:VSVdnH7cQ7V+B33qSJHTCRlNgra1607Q8PzEmnvb2Ic= Replace:<nil>}
&{Path:github.com/apparentlymart/go-cidr Version:v1.1.0 Sum:h1:2mAhrMoF+nhXqxTzSZMUzDHkLjmIHC+Zzn4tdgBZjnU= Replace:<nil>}
&{Path:github.com/aunum/log Version:v0.0.0-20200821225356-38d2e2c8b489 Sum:h1:DKOk8ZLAPnn4P/qTwGj5x5wAMqHmaE1oL4+nl1laIu8= Replace:<nil>}
&{Path:github.com/beorn7/perks Version:v1.0.1 Sum:h1:VlbKKnNfV8bJzeqoa4cOKqO6bYr3WgKZxO8Z16+hsOM= Replace:<nil>}
&{Path:github.com/briandowns/spinner Version:v1.16.0 Sum:h1:DFmp6hEaIx2QXXuqSJmtfSBSAjRmpGiKG6ip2Wm/yOs= Replace:<nil>}
&{Path:github.com/cespare/xxhash/v2 Version:v2.1.1 Sum:h1:6MnRN8NT7+YBpUIWxHtefFZOKTAPgGjpQSxqLNn0+qY= Replace:<nil>}
&{Path:github.com/containerd/containerd Version:v1.3.0 Sum:h1:xjvXQWABwS2uiv3TWgQt5Uth60Gu86LTGZXMJkjc7rY= Replace:<nil>}
&{Path:github.com/cpuguy83/go-md2man/v2 Version:v2.0.0 Sum:h1:EoUDS0afbrsXAZ9YQ9jdu/mZ2sXgT1/2yyNng4PGlyM= Replace:<nil>}
&{Path:github.com/davecgh/go-spew Version:v1.1.1 Sum:h1:vj9j/u1bqnvCEfJOwUhtlOARqs3+rkHYY13jYWTU97c= Replace:<nil>}
&{Path:github.com/docker/distribution Version:v2.7.1+incompatible Sum:h1:a5mlkVzth6W5A4fOsS3D2EO5BUmsJpcB+cRlLU7cSug= Replace:<nil>}
&{Path:github.com/docker/docker Version:v1.4.2-0.20190924003213-a8608b5b67c7 Sum:h1:Cvj7S8I4Xpx78KAl6TwTmMHuHlZ/0SM60NUneGJQ7IE= Replace:<nil>}
&{Path:github.com/docker/go-connections Version:v0.4.0 Sum:h1:El9xVISelRB7BuFusrZozjnkIM5YnzCViNKohAFqRJQ= Replace:<nil>}
&{Path:github.com/docker/go-units Version:v0.4.0 Sum:h1:3uh0PgVws3nIA0Q+MwDC8yjEPf9zjRfZZWXZYDct3Tw= Replace:<nil>}
&{Path:github.com/evanphx/json-patch Version:v4.11.0+incompatible Sum:h1:glyUF9yIYtMHzn8xaKw5rMhdWcwsYV8dZHIq5567/xs= Replace:<nil>}
&{Path:github.com/fatih/color Version:v1.12.0 Sum:h1:mRhaKNwANqRgUBGKmnI5ZxEk7QXmjQeCcuYFMX2bfcc= Replace:<nil>}
&{Path:github.com/ghodss/yaml Version:v1.0.0 Sum:h1:wQHKEahhL6wmXdzwWG11gIVCkOv05bNOh+Rxn0yngAk= Replace:<nil>}
&{Path:github.com/go-logr/logr Version:v0.4.0 Sum: Replace:0xc000a54640}
&{Path:github.com/gogo/protobuf Version:v1.3.2 Sum:h1:Ov1cvc58UF3b5XjBnZv7+opcTcQFZebYjWzi34vdm4Q= Replace:<nil>}
&{Path:github.com/golang/groupcache Version:v0.0.0-20210331224755-41bb18bfe9da Sum:h1:oI5xCqsCo564l8iNU+DwB5epxmsaqB+rhGL0m5jtYqE= Replace:<nil>}
&{Path:github.com/golang/protobuf Version:v1.5.2 Sum:h1:ROPKBNFfQgOUMifHyP+KYbvpjbdoFNs+aK7DXlji0Tw= Replace:<nil>}
&{Path:github.com/google/go-cmp Version:v0.5.6 Sum:h1:BKbKCqvP6I+rmFHt06ZmyQtvB8xAkWdhFyr0ZUNZcxQ= Replace:<nil>}
&{Path:github.com/google/gofuzz Version:v1.2.0 Sum:h1:xRy4A+RhZaiKjJ1bPfwQ8sedCA+YS2YcCHW6ec7JMi0= Replace:<nil>}
&{Path:github.com/google/uuid Version:v1.2.0 Sum:h1:qJYtXnJRWmpe7m/3XlyhrsLrEURqHRM2kxzoxXqyUDs= Replace:<nil>}
&{Path:github.com/googleapis/gax-go/v2 Version:v2.0.5 Sum:h1:sjZBwGj9Jlw33ImPtvFviGYvseOtDM7hkSKB7+Tv3SM= Replace:<nil>}
&{Path:github.com/googleapis/gnostic Version:v0.4.1 Sum: Replace:0xc000a54880}
&{Path:github.com/hashicorp/golang-lru Version:v0.5.4 Sum:h1:YDjusn29QI/Das2iO9M0BHnIbxPeyuCHsjMW+lJfyTc= Replace:<nil>}
&{Path:github.com/imdario/mergo Version:v0.3.12 Sum:h1:b6R2BslTbIEToALKP7LxUvijTsNI9TAe80pLWN2g/HU= Replace:<nil>}
&{Path:github.com/json-iterator/go Version:v1.1.11 Sum:h1:uVUAXhF2To8cbw/3xN3pxj6kk7TYKs98NIrTqPlMWAQ= Replace:<nil>}
&{Path:github.com/juju/fslock Version:v0.0.0-20160525022230-4d5c94c67b4b Sum:h1:FQ7+9fxhyp82ks9vAuyPzG0/vVbWwMwLJ+P6yJI5FN8= Replace:<nil>}
&{Path:github.com/kballard/go-shellquote Version:v0.0.0-20180428030007-95032a82bc51 Sum:h1:Z9n2FFNUXsshfwJMBgNA0RU6/i7WVaAegv3PtuIHPMs= Replace:<nil>}
&{Path:github.com/lithammer/dedent Version:v1.1.0 Sum:h1:VNzHMVCBNG1j0fh3OrsFRkVUwStdDArbgBWoPAffktY= Replace:<nil>}
&{Path:github.com/logrusorgru/aurora Version:v2.0.3+incompatible Sum:h1:tOpm7WcpBTn4fjmVfgpQq0EfczGlG91VSDkswnjF5A8= Replace:<nil>}
&{Path:github.com/mattn/go-colorable Version:v0.1.8 Sum:h1:c1ghPdyEDarC70ftn0y+A/Ee++9zz8ljHG1b13eJ0s8= Replace:<nil>}
&{Path:github.com/mattn/go-isatty Version:v0.0.12 Sum:h1:wuysRhFDzyxgEmMf5xjvJ2M9dZoWAXNNr5LSBS7uHXY= Replace:<nil>}
&{Path:github.com/mattn/go-runewidth Version:v0.0.7 Sum:h1:Ei8KR0497xHyKJPAv59M1dkC+rOZCMBJ+t3fZ+twI54= Replace:<nil>}
&{Path:github.com/matttproud/golang_protobuf_extensions Version:v1.0.2-0.20181231171920-c182affec369 Sum:h1:I0XW9+e1XWDxdcEniV4rQAIOPUGDq67JSCiRCgGCZLI= Replace:<nil>}
&{Path:github.com/mgutz/ansi Version:v0.0.0-20170206155736-9520e82c474b Sum:h1:j7+1HpAFS1zy5+Q4qx1fWh90gTKwiN4QCGoY9TWyyO4= Replace:<nil>}
&{Path:github.com/modern-go/concurrent Version:v0.0.0-20180306012644-bacd9c7ef1dd Sum:h1:TRLaZ9cD/w8PVh93nsPXa1VrQ6jlwL5oN8l14QlcNfg= Replace:<nil>}
&{Path:github.com/modern-go/reflect2 Version:v1.0.1 Sum:h1:9f412s+6RmYXLWZSEzVVgPGK7C2PphHj5RJrvfx9AWI= Replace:<nil>}
&{Path:github.com/olekukonko/tablewriter Version:v0.0.4 Sum:h1:vHD/YYe1Wolo78koG299f7V/VAS08c6IpCLn+Ejf/w8= Replace:<nil>}
&{Path:github.com/opencontainers/go-digest Version:v1.0.0 Sum:h1:apOUWs51W5PlhuyGyz9FCeeBIOUDA/6nW8Oi/yOhh5U= Replace:<nil>}
&{Path:github.com/opencontainers/image-spec Version:v1.0.1 Sum:h1:JMemWkRwHx4Zj+fVxWoMCFm/8sYGGrUVojFA6h/TRcI= Replace:<nil>}
&{Path:github.com/pkg/errors Version:v0.9.1 Sum:h1:FEBLx1zS214owpjy7qsBeixbURkuhQAwrK5UwLGTwt4= Replace:<nil>}
&{Path:github.com/prometheus/client_golang Version:v1.11.0 Sum:h1:HNkLOAEQMIDv/K+04rukrLx6ch7msSRwf3/SASFAGtQ= Replace:<nil>}
&{Path:github.com/prometheus/client_model Version:v0.2.0 Sum:h1:uq5h0d+GuxiXLJLNABMgp2qUWDPiLvgCzz2dUR+/W/M= Replace:<nil>}
&{Path:github.com/prometheus/common Version:v0.29.0 Sum:h1:3jqPBvKT4OHAbje2Ql7KeaaSicDBCxMYwEJU1zRJceE= Replace:<nil>}
&{Path:github.com/prometheus/procfs Version:v0.6.0 Sum:h1:mxy4L2jP6qMonqmq+aTtOx1ifVWUgG/TAmntgbh3xv4= Replace:<nil>}
&{Path:github.com/russross/blackfriday/v2 Version:v2.0.1 Sum:h1:lPqVAte+HuHNfhJ/0LC98ESWRz8afy9tM/0RK8m9o+Q= Replace:<nil>}
&{Path:github.com/shurcooL/sanitized_anchor_name Version:v1.0.0 Sum:h1:PdmoCO6wvbs+7yrJyMORt4/BmY5IYyJwS/kOiWx8mHo= Replace:<nil>}
&{Path:github.com/sirupsen/logrus Version:v1.8.1 Sum:h1:dJKuHgqk1NNQlqoA6BTlM1Wf9DOH3NBjQyu0h9+AZZE= Replace:<nil>}
&{Path:github.com/spf13/cobra Version:v1.1.3 Sum:h1:xghbfqPkxzxP3C/f3n5DdpAbdKLj4ZE4BWQI362l53M= Replace:<nil>}
&{Path:github.com/spf13/pflag Version:v1.0.5 Sum:h1:iy+VFUOCP1a+8yFto/drg2CJ5u0yRoB7fZw3DKv/JXA= Replace:<nil>}
&{Path:go.opencensus.io Version:v0.22.5 Sum:h1:dntmOdLpSpHlVqbW5Eay97DelsZHe+55D+xC6i0dDS0= Replace:<nil>}
&{Path:go.uber.org/atomic Version:v1.6.0 Sum:h1:Ezj3JGmsOnG1MoRWQkPBsKLe9DwWD9QeXzTRzzldNVk= Replace:<nil>}
&{Path:go.uber.org/multierr Version:v1.5.0 Sum:h1:KCa4XfM8CWFCpxXRGok+Q0SS/0XBhMDbHHGABQLvD2A= Replace:<nil>}
&{Path:golang.org/x/crypto Version:v0.0.0-20210616213533-5ff15b29337e Sum:h1:gsTQYXdTw2Gq7RBsWvlQ91b+aEQ6bXFUngBGuR8sPpI= Replace:<nil>}
&{Path:golang.org/x/mod Version:v0.4.2 Sum:h1:Gz96sIWK3OalVv/I/qNygP42zyoKp3xptRVCWRFEBvo= Replace:<nil>}
&{Path:golang.org/x/net Version:v0.0.0-20210614182718-04defd469f4e Sum:h1:XpT3nA5TvE525Ne3hInMh6+GETgn27Zfm9dxsThnX2Q= Replace:<nil>}
&{Path:golang.org/x/oauth2 Version:v0.0.0-20210628180205-a41e5a781914 Sum:h1:3B43BWw0xEBsLZ/NO1VALz6fppU3481pik+2Ksv45z8= Replace:<nil>}
&{Path:golang.org/x/sys Version:v0.0.0-20210910150752-751e447fb3d0 Sum:h1:xrCZDmdtoloIiooiA9q0OQb9r8HejIHYoHGhGCe1pGg= Replace:<nil>}
&{Path:golang.org/x/term Version:v0.0.0-20210615171337-6886f2dfbf5b Sum:h1:9zKuko04nR4gjZ4+DNjHqRlAJqbJETHwiNKDqTfOjfE= Replace:<nil>}
&{Path:golang.org/x/text Version:v0.3.6 Sum:h1:aRYxNxv6iGQlyVaZmk6ZgYEDa+Jg18DxebPSrd6bg1M= Replace:<nil>}
&{Path:golang.org/x/time Version:v0.0.0-20210611083556-38a9dc6acbc6 Sum:h1:Vv0JUPWTyeqUq42B2WJ1FeIDjjvGKoA2Ss+Ts0lAVbs= Replace:<nil>}
&{Path:gomodules.xyz/jsonpatch/v2 Version:v2.2.0 Sum:h1:4pT439QV83L+G9FkcCriY6EkpcK6r6bK+A5FBUMI7qY= Replace:<nil>}
&{Path:google.golang.org/api Version:v0.40.0 Sum:h1:uWrpz12dpVPn7cojP82mk02XDgTJLDPc2KbVTxrWb4A= Replace:<nil>}
&{Path:google.golang.org/genproto Version:v0.0.0-20201214200347-8c77b98c765d Sum:h1:HV9Z9qMhQEsdlvxNFELgQ11RkMzO3CMkjEySjCtuLes= Replace:<nil>}
&{Path:google.golang.org/grpc Version:v1.34.0 Sum:h1:raiipEjMOIC/TO2AvyTxP25XFdLxNIBwzDh3FM3XztI= Replace:<nil>}
&{Path:google.golang.org/protobuf Version:v1.27.1 Sum:h1:SnqbnDw1V7RiZcXPx5MEeqPv2s79L9i7BJUlG/+RurQ= Replace:<nil>}
&{Path:gopkg.in/fsnotify.v1 Version:v1.4.7 Sum:h1:xOHLXZwVvI9hhs+cLKq5+I5onOuwQLhQwiu63xxlHs4= Replace:<nil>}
&{Path:gopkg.in/inf.v0 Version:v0.9.1 Sum:h1:73M5CoZyi3ZLMOyDlQh031Cx6N9NDJ2Vvfl76EDAgDc= Replace:<nil>}
&{Path:gopkg.in/yaml.v2 Version:v2.4.0 Sum:h1:D8xgwECY7CYvx+Y2n4sBz93Jn9JRvxdiyyo8CTfuKaY= Replace:<nil>}
&{Path:k8s.io/api Version:v0.21.2 Sum: Replace:0xc000a55480}
&{Path:k8s.io/apiextensions-apiserver Version:v0.21.2 Sum:h1:+exKMRep4pDrphEafRvpEi79wTnCFMqKf8LBtlA3yrE= Replace:<nil>}
&{Path:k8s.io/apimachinery Version:v0.21.2 Sum: Replace:0xc000a55540}
&{Path:k8s.io/client-go Version:v0.21.2 Sum: Replace:0xc000a555c0}
&{Path:k8s.io/klog Version:v1.0.0 Sum:h1:Pt+yjF5aB1xDSVbau4VsWe+dQNzA0qv1LlXdC2dF6Q8= Replace:<nil>}
&{Path:k8s.io/klog/v2 Version:v2.8.0 Sum: Replace:0xc000a55680}
&{Path:k8s.io/kube-openapi Version:v0.0.0-20210305001622-591a79e4bda7 Sum: Replace:0xc000a55700}
&{Path:k8s.io/utils Version:v0.0.0-20210527160623-6fdb442a123b Sum:h1:MSqsVQ3pZvPGTqCjptfimO2WjG7A9un2zcpiHkA6M/s= Replace:<nil>}
&{Path:sigs.k8s.io/controller-runtime Version:v0.7.0 Sum: Replace:0xc000a557c0}
&{Path:sigs.k8s.io/yaml Version:v1.2.0 Sum:h1:kr/MCeFWJWTwyaHoR9c8EjH9OumOmoF9YGiZd7lFm/Q= Replace:<nil>}
```

As you can see, you can get information like
- Which version of `go` was used to build the binary
- What are the different Golang modules used in the binary

When running this program, you can cross verify the modules information in the output with the `go.mod` and `go.sum` files if you have access to the source code of the binary's project. In the above example, you can find the Golang modules files in the [Tanzu Framework open source project](https://github.com/vmware-tanzu/tanzu-framework/) in `v0.10.0` - [`go.mod`](https://github.com/vmware-tanzu/tanzu-framework/blob/v0.10.0/go.mod) , [`go.sum`](https://github.com/vmware-tanzu/tanzu-framework/blob/v0.10.0/go.sum)
