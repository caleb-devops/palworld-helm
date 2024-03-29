# Palworld Helm Chart

> [!IMPORTANT]  
> This chart has been moved to <https://github.com/caleb-devops/helm-charts>. Please remove this repository and add the new location.
>
> ```sh
> helm repo remove caleb-devops
> helm repo add caleb-devops https://caleb-devops.github.io/helm-charts
> ```

This chart installs [docker-palworld-dedicated-server](https://github.com/jammsen/docker-palworld-dedicated-server) on a [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager.

## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```sh
helm repo add caleb-devops https://caleb-devops.github.io/palworld-helm
```

Chart documentation is available in the [palworld directory](./charts/palworld/).
