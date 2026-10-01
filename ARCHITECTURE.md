# ARCHITECTURE — k8s-shopify-back-in-stock-pocharlies

Manifests de `shopify-back-in-stock` (avisos de stock y preventas). Código en `pocharlies-org/shopify-back-in-stock`.

## Clientes y versiones
- Servicios en ns `skirmshop`: app Remix, worker email, worker WhatsApp, Maddy SMTP; IngressRoute vía `traefik-edge` para `sauvage.e-dani.com`/`server.skirmshop.es` (`/apps/back-in-stock`, `/`, `/auth`, `/app`…). Tronco: `main` (Application `shopify-back-in-stock`, path `k8s`).

## Dependencias (ambos sentidos)
- Imágenes `harbor.lan.e-dani.com/homelab/shopify-back-in-stock`, `-worker-email`, `-worker-whatsapp` (por digest) y `foxcpp/maddy`. Secrets por `externalsecret.yaml`.
- Contratos que publica/consume la app: `CONTRACTS.yaml` del repo fuente.

## Stack
Kustomize + `manifest.yaml` plano; sin Helm; no usa la base del framework.

## Componentes compartidos
Ninguno propio.

## Cómo se construye
`k8s/manifest.yaml` (ConfigMap, 4 Deployments con sus Services, Middleware, IngressRoute), tres CronJobs con la imagen de la app: `backorder-enroll` (`15 */6 * * *`), `reconcile-coverage` (`3-59/10 * * * *`), `reconcile-holds` (`*/10 * * * *`).

## Tests y validaciones
`reusable-ci.yml` (yamllint/kustomize/kubeconform).

## CI/CD y despliegue
`ci.yml`, `pr-review.yml`; sin `release.yml` (los digests se editan en los manifests; no verificado quién). ArgoCD lee `main`.

## Decisiones y trampas
- Los CronJobs llevan digests propios, distintos entre sí: al subir imagen hay que actualizar los tres más el Deployment.
