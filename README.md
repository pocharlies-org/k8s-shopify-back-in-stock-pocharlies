# k8s-shopify-back-in-stock-pocharlies

Disabled k8s skeleton for `sauvage:/home/ubuntu/skirmshop/shopify-back-in-stock`.

The old route pointed at `sauvage.e-dani.com/apps/back-in-stock` on host port
3470, but that port was not listening during the audit. This skeleton creates
only a zero-replica Deployment, Service, and Vault-backed secret placeholder.
It intentionally creates no IngressRoute.

Activation steps:

1. Finish source repo readiness and build a production image.
2. Populate `secret/skirmshop/back-in-stock`.
3. Replace the `pending` image tag and validate `kustomize build k8s`.
4. Add the disabled ArgoCD Application, sync with `replicas: 0`, then test by
   temporarily scaling privately.
5. Add ingress only after Shopify OAuth URLs and rollback are confirmed.
