# fly a kite

## Overview

This is an opinionated take on a relatively simple kubernetes cluster. It can be used to stand up a k3s cluster
with automatic TLS encrypted services, using a wildcard domain and certificate. (e.g. https://*.sub.my.domain)

Features:

* Single host
* Let's Encrypt Wildcard certificate
* Simple TLS setup for applications
* Simple authentication setup for applications

## Setup

### Prerequisites

1. DigitalOcean managed DNS zone, with API token
1. DNS wildcard pointed to node IP
1. [Generic OAuth provider](https://github.com/thomseddon/traefik-forward-auth/wiki/Provider-Setup) information
1. SSH access as yourself to the node IP, with sudo access
1. [asdf](https://asdf-vm.com) installed (optional)
1. [mask](https://github.com/jakedeichert/mask) installed (or manually run the commands in maskfile.md)

### Environment Config

Set the following environment variables, I use [direnv](https://direnv.net):

```bash
# k3sup defaults to this
export KUBECONFIG=$KUBECONFIG:$PWD/kubeconfig
export FLY_ACME_EMAIL=adam@my.domain
# https://cloud.digitalocean.com/account/api/
export FLY_DO_TOKEN=
export FLY_SUBDOMAIN=sub.my.domain
# get these from your oauth provider
export FLY_OAUTH_AUTH_URL=
export FLY_OAUTH_TOKEN_URL=
export FLY_OAUTH_USER_URL=
export FLY_OAUTH_CLIENT_ID=
export FLY_OAUTH_CLIENT_SECRET=
# mask gen cookiesecret
export FLY_COOKIE_SECRET=
# mask gen signingsecret
export FLY_COOKIE_SIGNING_SECRET=
```

### Tools

I recommend asdf, but otherwise you can install the versions of tools listed in `.tool-versions`.

```bash
asdf install
```

## Install kubernetes and sync

1. install k3s: `mask bootstrap <node IP/name>`
1. verify cluster connection: `kubectl get nodes`
1. create the resources: `mask sync`
1. Open your browser and log in to the traefik dashboard: `https://proxy.sub.my.domain`
1. Check out kanboard at `https://kanboard.sub.my.domain`
