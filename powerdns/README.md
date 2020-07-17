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
| `powerdns.api.enabled` | Should the PowerDNS API be enabled | `true` |
| `powerdns.api.key` | PowerDNS API key | `PowerDNSAPI` |
| `powerdns.webserver.allowFrom` | PowerDNS webserver allowed IP whitelist | `0.0.0.0/0` |
| `powerdns.dnsupdate.enabled` | Should DNS UPDATE support be enabled | `false` |
| `powerdns.zoneConfig` | Used to specify an initial DNS zone configuration as SQL commands. | `nil` | 
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

### [Specifying Initial Zone Configuration](#zone-config)

You can specify an initial Zone configuration with SQL `INSERT ...` commands. This will require 2 statements. One to add the domain, the next to set the domains primary record. The following will create a sample initial zone called `my.sample.domain` with 60 second TTL and refresh settings. See PowerDNS [docs](https://doc.powerdns.com/authoritative/) for more info on record format.

```yaml
pdns:
  zoneConfig: |
    INSERT INTO domains (name, type, notified_serial) 
    VALUES ('my.sample.domain', 'MASTER', 24);
    
    INSERT INTO records (domain_id, name, type, content, ttl, prio)
    VALUES (1, 'my.sample.domain', 'SOA', 'k8s.powerdns.server hostmaster.my.sample.domain 24 60 60 60 60', 60, 0);
```

### Supporting TCP and UDP on the same LoadBalanced service

Kubernetes has limitations about allowing the same port to be used for both TCP and UDP protocols. Some LoadBalancer providers can get around this (ie: MetalLB) using service annotations. A potential MetalLB setup would be:

```yaml
service:
  type: LoadBalancer
  annotations:
    metallb.universe.tf/allow-shared-ip: powerdns 
```
