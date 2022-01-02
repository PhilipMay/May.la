# Helm
- https://helm.sh/
- https://artifacthub.io/

## Basic Commands
- install example: `helm upgrade --install loki grafana/loki-stack -n loki --create-namespace -f helm-loki.yaml`

## Search / List Commands
- list installed releases (all namespaces): `helm list -A`
- search the repo for latest version: `helm search repo <reponame>/<chartname>`
- list all versions from repo: `helm search repo <reponame>/<chartname> -l`
