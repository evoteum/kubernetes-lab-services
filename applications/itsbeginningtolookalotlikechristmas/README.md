# itsbeginningtolookalotlikechristmas

Deploys the static site for [ItsBeginningToLookALotLike.Christmas](https://ItsBeginningToLookALotLike.Christmas) — a 3-pod nginx `Deployment` (HPA up to 10), exposed via the `cloudflare-gateway` Gateway API.

The Helm chart lives in the application's own repo under `chart/`; this `Application` points ArgoCD at that path.

Traffic is routed by the `cloudflare-gateway` `HTTPRoute` (`httproute.yaml`). The site image is rebuilt automatically whenever `data.csv` or static files change, triggered by [itsbeginningtolookalotlikechristmas-datagather](../itsbeginningtolookalotlikechristmas-datagather/).

## Required secrets

Sourced from OpenBao via the `openbao` `ClusterSecretStore`.

| OpenBao path                                         | Key           | Value                 |
|------------------------------------------------------|---------------|-----------------------|
| `itsbeginningtolookalotlikechristmas/spotify`        | `client_id`   | Spotify app client ID |
| `itsbeginningtolookalotlikechristmas/spotify`        | `client_secret` | Spotify app client secret |
