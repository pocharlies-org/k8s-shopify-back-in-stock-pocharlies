# k8s-shopify-back-in-stock-pocharlies

GitOps deployment for `shopify-back-in-stock`, the Shopify embedded app named
`Skirmshop Stock & Preorders`.

Routing contract:

1. Shopify `application_url` is the canonical origin `https://sauvage.e-dani.com`.
2. Remix owns `/` and redirects to `/app`, preserving Shopify query params.
3. Remix owns `/app`, `/auth`, `/api`, `/webhooks`, `/__manifest`, `/healthz`,
   and `/readyz`.
4. The Shopify App Proxy owns `/apps/back-in-stock` and Traefik strips that
   prefix before forwarding to the app.
5. WhatsApp must not be the fallback for the Shopify app root. WhatsApp has its
   own hosts (`whatsapp.e-dani.com`, `whatsapp-open.e-dani.com`, and LAN
   aliases).

Keep this route contract aligned with:

- `shopify-back-in-stock/shopify.app.toml`
- `shopify-back-in-stock/shopify.app.back-in-stock-notifications.toml`
- `shopify-back-in-stock/scripts/validate-production-config.ts`
- `k8s-infra-pocharlies/networking/traefik-edge/legacy-public-routes.yaml`
