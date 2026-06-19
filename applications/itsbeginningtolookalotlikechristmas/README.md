# itsbeginningtolookalotlikechristmas

Deploys two workloads for [itsbeginningtolookalotlikechristmas](https://github.com/evoteum/itsbeginningtolookalotlikechristmas):

- **datagather** — daily `CronJob` that fetches the Spotify popularity score and pushes `data.csv` back to GitHub.
- **site** — 3-pod nginx `Deployment` (HPA up to 10) serving the static site, with a 2-pod `cloudflared` sidecar exposing it via Cloudflare Tunnel.

The Helm chart lives in the application's own repo under `chart/`; this `Application` points ArgoCD at that path.

## Required secrets

Sourced from OpenBao via the `openbao` `ClusterSecretStore`. Run [`init-secrets.sh`](init-secrets.sh) to create the paths — nothing is committed to git.

| OpenBao path                                           | Key              | Value                        |
|--------------------------------------------------------|------------------|------------------------------|
| `itsbeginningtolookalotlikechristmas/spotify`          | `client_id`      | Spotify app client ID        |
| `itsbeginningtolookalotlikechristmas/spotify`          | `client_secret`  | Spotify app client secret    |
| `itsbeginningtolookalotlikechristmas/git-deploy-key`   | `ssh_privatekey` | SSH deploy key (private key) |
| `itsbeginningtolookalotlikechristmas/cloudflare-tunnel`| `token`          | Cloudflare Tunnel token (retrieve after first tunnel sync — see tunnel README) |

The deploy key's public half must be added to the GitHub repo (Settings → Deploy keys) with **write access** enabled.

The Cloudflare Tunnel is managed by Crossplane in `kubernetes-lab-config/platform/tunnel/itsbeginningtolookalotlikechristmas/`; see the README there for platform-level secrets (`cloudflare-tunnel-secret`, `cloudflare/api-token`).
