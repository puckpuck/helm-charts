# kspan

Installs and configures [kspan](https://github.com/puckpuck/kspan) to get tracing from your Kubernetes events

**kspan** is a library to generate traces from Kubernetes events and sends them to an OpenTelemetry protocol (OTLP)
compatible backend.

> Kubernetes is an application used to manage your workloads. So let's trace it like an application.

## Installing the Chart

You will likely need to configure multiple `args` and optionally the `env` parameter as well. These are array style
parameters which are best configured within a custom values file.

The configuration will be based on the specific OTLP enabled backend that you are using. This example demonstrates how
to configure for use with [Honeycomb.io](https://honeycomb.io) by referencing an existing secret for the API key.

```yaml
args:
  - --otlp-addr=api.honeycomb.io:443
  - --otlp-headers=x-honeycomb-team=$(HONEYCOMB_API_KEY),x-honeycomb-dataset=kubernetes-traces
  - --otlp-secured

env:
  - name: HONEYCOMB_API_KEY
    valueFrom:
      secretKeyRef:
        name: honeycomb
        key: api-key
```

With a values file specified, you can install the chart

```shell
helm repo add puckpuck https://puckpuck.github.io/helm-charts
helm install kspan puckpuck/kspan --values my-values.yaml
```

## Configuration

The [values.yaml](values.yaml) file contains information about all configuration options for this chart.

## Parameters

| Parameter | Description | Default |
| --- | --- | --- |
| `args` | Array of command arguments to pass into the kspan process | `[]` |
| `env` | Array of environment variables to set on the kspan pod | `[]` |
| `image.repository` | PowerDNS Image repository | `psitrax/powerdns` |
| `image.tag` | PowerDNS Image tag (leave blank to use app version) | `nil` |
| `image.pullPolicy` | PowerDNS Image pull policy | `IfNotPresent` |
| `resources` | CPU/Memory resource limits/requests | `{}` |
| `nodeSelector` | Node labels for pod assignment | `{}` |
| `tolerations` | Toleration labels for pod assignment | `[]` |
| `affinity` | Affinity settings for pod assignment | `{}` |