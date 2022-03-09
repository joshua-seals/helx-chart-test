# helx

A Helm chart for deploying HeLx to Kubernetes.

![Version: 0.13.2](https://img.shields.io/badge/Version-0.13.2-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.9.0](https://img.shields.io/badge/AppVersion-1.9.0-informational?style=flat-square)

HeLx puts the most advanced analytical scientific models at investigator’s finger tips using equally advanced cloud native, container orchestrated, distributed computing systems. HeLx can be applied in many domains. Its ability to empower researchers to leverage advanced analytical tools without installation or other infrastructure concerns has broad reaching benefits.

```
# The most basic deployment of HeLx to a Kubernetes cluster on GKE.
NAMESPACE=helx
# Add the helxplatform Helm repository.
helm repo add helx-charts https://helxplatform.github.io/helm-charts
# Pull down latest chart updates.
helm repo update
helm -n $NAMESPACE --create-namespace install helx helx-charts/helx

# Deploy to a non-GKE cluster.
helm -n $NAMESPACE --create-namespace install helx helx-charts/helx --set appstore.userStorage.createPVC=true,nfs-server.enabled=false

# Review the output of the Helm install command.  To review the output use the
# status option.
helm -n $NAMESPACE status helx
# Delete the HeLx chart.
helm -n $NAMESPACE delete helx
# Get the default values yaml for HeLx and subcharts.
helm inspect values helx-charts/[helx ambassador nginx etc.]

```

To do more than the most basic install you should create a values.yaml that contains settings for your local HeLx environment.  A sample is below.

```
appstore:
  django:
    APPSTORE_DJANGO_PASSWORD: "< my secret password >"
    AUTHORIZED_USERS: "user1@example.com,user2@example.com,user3@example.com"
  ACCOUNT_DEFAULT_HTTP_PROTOCOL: https
  userStorage:
    createPVC: true
  oauth:
    OAUTH_PROVIDERS: "google,github"
    GOOGLE_NAME: "< secret >"
    GOOGLE_CLIENT_ID: "< secret >"
    GOOGLE_SECRET: "< secret >"
    GITHUB_NAME: "< secret >"
    GITHUB_CLIENT_ID: "< secret >"
    GITHUB_SECRET: "< secret >"

nfs-server:
  enabled: false

nginx:
  service:
    serverName: helx.example.com
  SSL:
    nginxTLSSecret: example-tls-secret
```

To deploy HeLx using the values.yaml use the following command.
```
helm -n $NAMESPACE --create-namespace install helx helx-charts/helx --values values.yaml
```

You can view the README.md files for each subchart to see the variables that exist.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| ambassador.enabled | bool | `true` | enable/disable deployment of Ambassador |
| appstore.enabled | bool | `true` | enable/disable deployment of appstore |
| backup-pvc-cronjob.enabled | bool | `false` | enable/disable deployment of backup-pvc-cronjob |
| global.redis.existingSecret | string | `"redis-secret"` |  |
| global.redis.existingSecretPasswordKey | string | `"redis-password"` |  |
| global.restartr_api_service_name | string | `"helx-restartr-api-service"` |  |
| global.stdnfsPvc | string | `"stdnfs"` |  |
| global.tycho_api_service_name | string | `"helx-tycho-api"` |  |
| image-utils.enabled | bool | `false` | enable/disable deployment of image-utils (imagepullsecret-patcher and imagepuller) |
| monitoring.enabled | bool | `false` | enable/disable deployment of monitoring (kube-prometheus-stack, cost-analyzer, etc.) |
| nfs-server.enabled | bool | `true` | enable/disable deployment of nfs-server |
| nfsrods.enabled | bool | `false` | enable/disable deployment of nfsrods |
| nginx.enabled | bool | `true` | enable/disable deployment of nginx |
| pod-reaper.enabled | bool | `true` | enable/disable deployment of pod-reaper |
| search.enabled | bool | `false` | enable/disable deployment of search |
| tycho-api.enabled | bool | `false` | enable/disable deployment of tycho-api |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.5.0](https://github.com/norwoodj/helm-docs/releases/v1.5.0)
