# planzoco

Deploys [planzo.co](https://planzo.co) — a small Gin/Go voting app, backed by a CloudNativePG `Cluster` (`planzoco-database`), exposed via the `cloudflare-gateway` Gateway API.

The Helm chart lives in the application's own repo under `chart/`; this `Application` points ArgoCD at that path. The app reads its connection string from the `planzoco-database-app` Secret's `uri` key, which CNPG generates automatically once `cnpg-cluster.yaml` syncs, no manual secret setup needed.

Traffic is routed by the `cloudflare-gateway` `HTTPRoute` (`httproute.yaml`), hostname `planzo.co`.

