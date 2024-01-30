# Palworld Helm Chart

This chart installs a Palworld deployment on a [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager.

## Install Chart

```sh
helm install [-n NAMESPACE] RELEASE_NAME .
```

## Uninstall Chart

```sh
helm uninstall [-n NAMESPACE] RELEASE_NAME
```

## Upgrading Chart

```sh
helm upgrade [-n NAMESPACE] RELEASE_NAME .
```

## Configuration

See [Customizing the Chart Before Installing](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing). To see all configurable options with detailed comments, visit the chart's [values.yaml](./values.yaml).

### Networking

This Chart uses a [LoadBalancer Service](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer) to expose the application outside Kubernetes:

```yaml
service:
  type: LoadBalancer
  ## Example annotations for kube-vip or MetalLB to assign an IP to the LoadBalancer
  annotations: {}
    # kube-vip.io/loadbalancerIPs: 192.168.1.100
    # metallb.universe.tf/loadBalancerIPs: 192.168.1.100
  port: 8211
```

Note: Port-Forwarding may be required if the LoadBalancer does not use a Public IP.

### Persistent Volume

Data persistance is enabled by default using dynamic volume provisioning.

**Set the configured storageClass [reclaim policy](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy) to `Retain` to prevent automatic deletion of the PV if the Chart is uninstalled.**

### Game config

To configure the server, add the [environment variables](https://github.com/jammsen/docker-palworld-dedicated-server#environment-variables) to `config`:

```yaml
config:
  env:
    TZ: UTC # Change this for logging and backup
    ALWAYS_UPDATE_ON_START: true
    MAX_PLAYERS: 16
    MULTITHREAD_ENABLED: true
    COMMUNITY_SERVER: false
    RCON_ENABLED: true
    SERVER_NAME: "serverNameHere"
    SERVER_DESCRIPTION: ""
    BACKUP_ENABLED: true
    BACKUP_CRON_EXPRESSION: 0 * * * *
    PUBLIC_IP: ""
    PUBLIC_PORT: 8211
  
  secretEnv:
    create: true
    env:
      SERVER_PASSWORD: "serverPasswordHere"
      ADMIN_PASSWORD: "adminPasswordHere"
```
