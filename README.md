# Mastodon chart

A Helm chart for [Mastodon](https://github.com/tootsuite/mastodon).
Mastodon is a free, open-source social network server.

This Helm chart is designed/tested with:

| Package | Version |
| ------- | ------- |
| Mastodon | `tootsuite/mastodon:v3.3.0` |
| Kubernetes | v1.23 |

# Installation

Copy and edit `secrets.yaml.sample` and `values.yaml.sample` to provide your
own `secrets.yaml` and `values.yaml` files, and then deploy the Helm chart
with: 

```
helm install -f secrets.yaml mastodon .
```

# About

This helm chart is tweaked for my own infrastructure services and you'll likely need to do the same for your own setup, depending on
what cloud you are running in and what cluster services you have set up. 

In particular, it expects a working `cert-manager` cluster service, and expects that you are using Traefik as your ingress controller. If you are
using either of the nginx ingress controllers or some other mechanism, you will need to edit the Ingress resources appropriately.

The original version of this chart expected that you would use `ReadWriteMany` persistent volumes, mostly for no reason. Most CSI drivers do not 
support ReadWriteMany persistent volumes. This version of the chart uses only `ReadWriteOnce` volumes.

### Maintainer

Maintained by William Lahti <wilahti@gmail.com>.

Previously maintained by Tim Walls <tim.walls@snowgoons.com> - for more information see
here: https://snowgoons.ro/posts/2020-08-29-mastodon-on-kubernetes/

Based on the original chart developed by Ladicle - https://github.com/Ladicle/mastodon-chart
