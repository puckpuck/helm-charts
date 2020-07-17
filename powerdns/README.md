# Helm Chart for PowerDNS

Installs a PowerDNS authoritative nameserver inside a Kubernetes cluster

## TL;DR;

```bash
helm repo add puckpuck https://puckpuck.github.io/helm-charts
helm install powerdns puckpuck/powerdns
```

> Note: This will leave you with a PowerDNS server deployed to your k8s cluster. You will also need to configure some means of accesibility from the outside world for the DNS server to be accesible. You can do it by setting your service type to LoadBalancer and using annotations to ensure both TCP/UDP can be accessed.


## Configuration

The [values.yaml](./values.yaml) file contains information about all configuration
options for this chart.

## Parameters

| Parameter | Description | Default |
| --- | --- | --- |
| `powerdns.api.key` | PowerDNS API key | `PowerDNSAPI` |
| `powerdns.initDomains` | List of domains to configure on startup | `[]` | 
| `replicaCount` | Number of pdns nodes | `1` |
| `image.repository` | PowerDNS Image repository | `psitrax/powerdns` |
| `image.tag` | PowerDNS Image tag (leave blank to use app version) | `nil` |
| `image.pullPolicy` | PowerDNS Image pull policy | `IfNotPresent` |
| `service.type` | Kubernetes service type | `ClusterIP` |
| `service.annotations` | Annotations for PowerDNS service | `{}` | 
| `service.ip` | Specify IP address for PowerDNS service | `nil` |
| `resources` | CPU/Memory resource limits/requests | `{}` |
| `nodeSelector` | Node labels for pod assignment | `{}` |
| `tolerations` | Toleration labels for pod assignment | `[]` |
| `affinity` | Affinity settings for pod assignment | `{}` |
| `mariadb.enabled` | Deploy MariaDB container(s) | `true` |
| `mariadb.rootUser.password` | MariaDB admin password | `nil` |
| `mariadb.db.name` | Database name to create | `powerdns` |
| `mariadb.db.user` | Database user to create | `powerdns` |
| `mariadb.db.password` | Password for the database | `powerdns` |

### Supporting TCP and UDP on the same LoadBalanced service

Kubernetes has limitations about allowing the same port to be used for both TCP and UDP protocols. Some LoadBalancer providers can get around this (ie: MetalLB) using service annotations. A potential MetalLB setup would be:

```yaml
service:
  type: LoadBalancer
  annotations:
    metallb.universe.tf/allow-shared-ip: powerdns 
```
