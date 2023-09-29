Commands
====

Upgrade (dev): `helm diff upgrade starmatch-nginx-dev bitnami/nginx -f values/dev.values.yaml --version 8.2.7`

Upgrade (live): `helm diff upgrade starmatch-nginx-live bitnami/nginx -f values/live.values.yaml --version 8.2.7`
