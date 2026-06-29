# itsbeginningtolookalotlikechristmas-datagather

Daily `CronJob` that fetches the Spotify popularity score for [ItsBeginningToLookALotLike.Christmas](https://ItsBeginningToLookALotLike.Christmas) and pushes `data.csv` to the [itsbeginningtolookalotlikechristmas](https://github.com/evoteum/itsbeginningtolookalotlikechristmas) repo, triggering a site image rebuild.

The Helm chart lives in the application's own repo under `chart/`; this `Application` points ArgoCD at that path.

## Required secrets

Sourced from OpenBao via the `openbao` `ClusterSecretStore`.

| OpenBao path                                         | Key              | Value                        |
|------------------------------------------------------|------------------|------------------------------|
| `itsbeginningtolookalotlikechristmas/spotify`        | `client_id`      | Spotify app client ID        |
| `itsbeginningtolookalotlikechristmas/spotify`        | `client_secret`  | Spotify app client secret    |
| `platform/evoteumbot`                                | `ssh_privatekey` | SSH deploy key (private key) |

The deploy key's public half must be added to the
[itsbeginningtolookalotlikechristmas](https://github.com/evoteum/itsbeginningtolookalotlikechristmas)
repo (Settings → Deploy keys) with **write access** enabled.
