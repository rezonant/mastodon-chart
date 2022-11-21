# Mastodon chart

An easy-to-use horizontally scalable Helm chart for [Mastodon](https://github.com/tootsuite/mastodon).
Mastodon is an open-source decentralized microblogging social network.

This Helm chart is designed/tested with:

| Package | Version |
| ------- | ------- |
| Mastodon | `tootsuite/mastodon:v3.3.0` |
| Kubernetes | v1.23 |

# Prerequisites

- A Kubernetes cluster running v1.23+
- An SMTP relay service 
    * An option: If you pay for Google Workspace you can send as a Workspace user on a custom domain
- An S3 bucket and custom domain for serving media (images, cached content, etc)
    * This can get very large! You will want to pick a cost effective S3 option! It took about 2 weeks to hit 60GB for 
      me :-)

# Important Notes

This helm chart is tweaked for my own cluster infrastructure services and you'll likely need to do the same for your 
own setup, depending on what cloud you are running in and what cluster services you have set up. 

In particular, it expects a working `cert-manager` cluster service for acquiring TLS certificates, and expects that you 
are using Traefik as your ingress controller. If you are using either of the nginx ingress controllers or something else, 
you will need to edit the Ingress resources appropriately.

Note that the chart will install its own PostgreSQL and Redis servers as single-instance (non-replicated) Deployments. 
If you want to use StatefulSets, primary/secondary replication, a managed database service, or anything else, you will 
need to customize the Helm chart to do so.  

# Installation

Check out this repository, then copy `secrets.yaml.sample` to `secrets.yaml` and `values.yaml.sample` to `values.yaml`,
and fill in the necessary details.

Note that you will probably want to leave single-user mode *enabled* to begin with while you get everything set up, and
to make it easy to create your admin account.

Once you are ready, deploy the Helm chart with: 

```
helm install -f secrets.yaml mastodon .
```

Note that `values.yaml` is the default values file for the chart and `secrets.yaml` "overrides" it, but in this chart they are actually unrelated, and both are required in order to install a release.

After it's up and you've made your first account, you can make that user an admin by running the following from 
within any of the web or sidekiq pods:

```
tootctl accounts modify YOUR_USERNAME --role=admin
```

For more information about `tootctl` (and lots of great information about running your new Mastodon instance), see
https://docs.joinmastodon.org/admin/tootctl/

After you have become Admin, you should then disable single user mode. You must do this by changing the values.yaml and then *upgrading* your Helm release.

I recommend acquiring the `helm-diff` plugin and always running a diff to see what you are about to change before running `helm upgrade`:

```
helm diff upgrade -f secrets.yaml mastodon .
```

Once you are ready, upgrade with

```
helm upgrade -f secrets.yaml mastodon .
```

### Maintainer

Maintained by William Lahti <wilahti@gmail.com>.

Previously maintained by Tim Walls <tim.walls@snowgoons.com> - for more information see
here: https://snowgoons.ro/posts/2020-08-29-mastodon-on-kubernetes/

Based on the original chart developed by Ladicle - https://github.com/Ladicle/mastodon-chart
