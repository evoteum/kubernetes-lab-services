# itsbeginningtolookalotlikechristmas

Runs the daily Spotify popularity poll for [itsbeginningtolookalotlikechristmas](https://github.com/evoteum/itsbeginningtolookalotlikechristmas)
as an in-cluster `CronJob`, replacing the GitHub Actions scheduled workflow (which was unreliable
about actually firing on schedule). The job clones the application repo, runs `src/spotify.py`,
and pushes `data.csv` straight back to GitHub so the data stays in git.

Manifests live in the application's own repo under `manifests/`; this `Application` just points
ArgoCD at that path.

## Required secrets

Secrets are sourced from OpenBao via the `openbao` `ClusterSecretStore` and synced into the
`itsbeginningtolookalotlikechristmas` namespace by the `ExternalSecret` resources in
[`manifests/externalsecrets.yaml`](https://github.com/evoteum/itsbeginningtolookalotlikechristmas/blob/main/manifests/externalsecrets.yaml).
Nothing is committed to git — populate these paths/keys in OpenBao before the CronJob can run:

| OpenBao path                                      | Key              | Value                              |
|----------------------------------------------------|------------------|-------------------------------------|
| `itsbeginningtolookalotlikechristmas/spotify`       | `client_id`      | Spotify app client ID                |
| `itsbeginningtolookalotlikechristmas/spotify`       | `client_secret`  | Spotify app client secret            |
| `itsbeginningtolookalotlikechristmas/git-deploy-key`| `ssh_privatekey` | SSH private key (deploy key)         |

The corresponding public key must be added to the `itsbeginningtolookalotlikechristmas` GitHub
repo (Settings → Deploy keys) with **write access** enabled.
