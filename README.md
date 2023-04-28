# k3d-boot

Linux and macOS script to create a k3d (k3s in docker) cluster for development
including:

- [Calico](https://github.com/cilium/cilium) networking and network security solution for Kubernetes
- [Ingress-NGINX](https://github.com/kubernetes/ingress-nginx) ingress controller for Kubernetes using NGINX
- [MetalLB](https://github.com/metallb/metallb) network load-balancer implementation

## Requirements

A working `docker` installation is required. Additional tooling will be downloaded automatically if they are not
available: `helm`, `k3d` and `kubectl`.

### macOS notes

Docker Desktop for Mac does not support routing to containers by IP address meaning that cluster nodes and load balancer
addresses cannot be accessed directly. This functionality is supported natively by Linux and requires additional tooling
on macOS. One such utility is [docker-mac-net-connect](https://github.com/chipmk/docker-mac-net-connect) which can be
installed via [homebrew](https://brew.sh/):

```sh
brew install chipmk/tap/docker-mac-net-connect
brew services start chipmk/tap/docker-mac-net-connect
```

## Quick Start

Use `./create.sh` to create the cluster. Once started the following ports will
be accessible via localhost:

- `8080` Ingress Controller HTTP port
- `8443` Ingress Controller HTTPS port
- `6443` Kubernetes API server

To configure the cluster use the command-line options:

```
Usage: 
  ./create.sh [options]

Options:
  -n, --cluster-name <>        cluster name (default: "default")
  -c, --cni <>                 cni plugin: "calico" | "flannel" (default: "calico")
  -l, --load-balancer <>       load balancer implementation: "metallb" | "servicelb" (default: "metallb")
  -a, --api-port <>            server api port (default: 6443)
  -P, --proxy-protocol         enable proxy protocol for ingress communication
  -p, --proxy-http-port <>     ingress http port to expose on host (default: 8080)
  -t, --proxy-tls-port <>      ingress tls port to expose on host (default: 8443)
  -L, --proxy-no-labels        do not add traefik labels to the proxy container
  -d, --proxy-host <>          traefik router host (e.g. k3s.localhost)
  -k, --proxy-certresolver <>  traefik certResolver (default: "letsencrypt")
  -e, --proxy-entrypoint <>    traefik entryPoint (default: "websecure")
  -s, --proxy-service <>       traefik service name (default: "k3s-${cluster_name}")
  -h, --help                   display help
```

To delete the cluster, run `./destroy.sh`.
