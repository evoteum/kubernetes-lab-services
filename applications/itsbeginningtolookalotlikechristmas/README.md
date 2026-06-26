# itsbeginningtolookalotlikechristmas

Deploys two workloads for [itsbeginningtolookalotlikechristmas](https://github.com/evoteum/itsbeginningtolookalotlikechristmas):

- **datagather** — daily `CronJob` that fetches the Spotify popularity score and pushes `data.csv` back to GitHub.
- **site** — 3-pod nginx `Deployment` (HPA up to 10) serving the static site, exposed via the `cloudflare-gateway` Gateway API.

The Helm chart lives in the application's own repo under `chart/`; this `Application` points ArgoCD at that path.

Traffic is routed by the `cloudflare-gateway` `HTTPRoute` (`httproute.yaml`) — no in-cluster `cloudflared` pod required.

## Required secrets

Sourced from OpenBao via the `openbao` `ClusterSecretStore`.

| OpenBao path                                         | Key              | Value                        |
|------------------------------------------------------|------------------|------------------------------|
| `itsbeginningtolookalotlikechristmas/spotify`        | `client_id`      | Spotify app client ID        |
| `itsbeginningtolookalotlikechristmas/spotify`        | `client_secret`  | Spotify app client secret    |
| `itsbeginningtolookalotlikechristmas/git-deploy-key` | `ssh_privatekey` | SSH deploy key (private key) |

The deploy key's public half must be added to the GitHub repo (Settings → Deploy keys) with **write access** enabled.
