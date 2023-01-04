# seashell

Installs and configures [seashell](https://github.com/puckpuck/seashell) to
provide tools for debugging Kubernetes workloads.

## Installing the chart

This chart should run without the need to specify any custom chart values.

```bash
helm repo add puckpuck https://puckpuck.github.io/helm-charts
helm install seashell puckpuck/seashell
```

## Parameters

> **Warning**
> Setting `rbac.readAll` to true will grant read access to ALL resources in your Kubernetes cluster. 
> This isn't recommended for anything that even comes close to a production environment. 
> Please know what the fuck you are doing before you deploy this thing blindly to your Kubernetes cluster.

| Parameter                    | Description                                                            | Default                                          |
|------------------------------|------------------------------------------------------------------------|--------------------------------------------------|
| `image.repository`           | seashell Image repository                                              | `puckpuck/seashell`                              |
| `image.tag`                  | seashell Image tag (leave blank to use app version)                    | `nil`                                            |
| `image.pullPolicy`           | seashell Image pull policy                                             | `IfNotPresent`                                   |
| `extraVolumeMounts`          | Additional volume mounts to add to the container                       | `[]`                                             |
| `extraVolumes`               | Additional volumes to add to the pod                                   | `[]`                                             |
| `serviceAccount.create`      | Specify whether a ServiceAccount should be created                     | `true`                                           |
| `serviceAccount.annotations` | Annotations to be applied to ServiceAccount                            | `{}`                                             |
| `serviceAccount.name`        | The name of the ServiceAccount to create                               | Generated using the `seashell.fullname` template |
| `rbac.create`                | Specify whether RBAC resources should be created and used              | `true`                                           |
| `rbac.readAll`               | Specify whether to include READ permssions on ALL Kubernetes resources | `false`                                          |
| `rbac.extraRules`            | Additional rules to add to the ClusterRole                             | `[]`                                             |
| `podAnnotations`             | Pod annotations                                                        | `{}`                                             |
| `podSecurityContext`         | Security context for pod                                               | `{}`                                             | 
| `securityContext`            | Security context for container                                         | `{}`                                             | 
| `resources`                  | CPU/Memory resource requests/limits                                    | `{}`                                             | 
| `nodeSelector`               | Node labels for pod assignment                                         | `{}`                                             | 
| `tolerations`                | Tolerations for pod assignment                                         | `[]`                                             | 
| `affinity`                   | Map of node/pod affinities                                             | `{}`                                             |
