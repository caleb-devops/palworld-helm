# Palworld Helm Chart

This chart installs [docker-palworld-dedicated-server](https://github.com/jammsen/docker-palworld-dedicated-server) on a [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager.

## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```sh
helm repo add caleb-devops https://caleb-devops.github.io/palworld-helm
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
caleb-devops` to see the charts.

To install the palworld chart:

```sh
helm install palworld caleb-devops/palworld
```

To upgrade the palworld chart:

```sh
helm upgrade palworld caleb-devops/palworld
```

To uninstall the chart:

```sh
helm uninstall palworld
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

> [!WARNING]
> Set the configured storageClass [reclaim policy](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy) to `Retain` to prevent automatic deletion of the PV if the chart is uninstalled.

### Palworld Config

To configure the server, add the [environment variables](https://github.com/jammsen/docker-palworld-dedicated-server/blob/master/README_ENV.md#environment-variables) to `config`:

```yaml
config:
  env:
    TZ: UTC                              # Timezone used for time stamping server backups
    ALWAYS_UPDATE_ON_START: true
    MAX_PLAYERS: 32
    MULTITHREAD_ENABLED: true
    COMMUNITY_SERVER: false
    SERVER_NAME: "Default Palworld Server"
    SERVER_DESCRIPTION: ""
    BACKUP_ENABLED: true
    BACKUP_CRON_EXPRESSION: "0 * * * *"  # Backup every hour
    BACKUP_RETENTION_POLICY: true        # Cleanup old backups
    BACKUP_RETENTION_AMOUNT_TO_KEEP: 168 # Retain backups for 7 days
    PUBLIC_IP: ""
    PUBLIC_PORT: 8211
  
  secretEnv:
    create: true
    env:
      SERVER_PASSWORD: "serverPasswordHere"
      ADMIN_PASSWORD: "adminPasswordHere"
```
