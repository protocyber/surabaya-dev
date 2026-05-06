# surabaya-dev

Shared development infrastructure for Surabaya City Government projects.

## Repository layout

- `shared-services/` contains shared Docker services deployed via Portainer stack.
- `traefik/docker-compose.yml` contains the Traefik stack compose used by Portainer.
- `traefik/dynamic/` contains Traefik file provider config (`services.yaml`, `tls.yaml`).
- `traefik/certs/` is for local certificates and keys (not committed).

## Shared services stack

Path: `shared-services/docker-compose.yml`

### Local testing in Portainer (recommended while tuning)

1. Open Portainer -> Stacks -> Add stack.
2. Paste or upload `shared-services/docker-compose.yml`.
3. Add environment variables from `shared-services/stack.env` values.
4. Deploy stack.

### Git mode in Portainer (when stable)

1. Push this repository to GitHub/GitLab/Gitea.
2. In Portainer, create stack from Repository.
3. Set compose path to `shared-services/docker-compose.yml`.
4. Set env vars in Portainer from your `stack.env` values.
5. Deploy and optionally enable GitOps polling/webhook updates.

## Traefik config

Paths:

- `traefik/docker-compose.yml` (Traefik stack)
- `traefik/dynamic/` (file provider config)

Current config is copied from `~/.config/traefik` and should be treated as source-controlled dynamic configuration.

To make this repo authoritative, update Traefik container bind mounts to use this repo:

- Dynamic config mount: `~/projects/surabaya-dev/traefik/dynamic:/etc/traefik`
- Certs mount: `~/projects/surabaya-dev/traefik/certs:/etc/traefik/certs`

After changing mounts, redeploy Traefik.

## Traefik stack deployment in Portainer

Path: `traefik/docker-compose.yml`

### Local testing in Portainer

1. Open Portainer -> Stacks -> Add stack.
2. Paste or upload `traefik/docker-compose.yml`.
3. Deploy stack.

### Git mode in Portainer

1. Push this repository to GitHub/GitLab/Gitea.
2. In Portainer, create stack from Repository.
3. Set compose path to `traefik/docker-compose.yml`.
4. Deploy and optionally enable GitOps polling/webhook updates.

## HTTPS routing behavior

Shared services exposed through Traefik labels are configured with:

- `web` router + `redirect-https@file` middleware for HTTP -> HTTPS redirect.
- `websecure` router with `tls=true` for HTTPS endpoint.
- `traefik.docker.network=shared-net` for explicit network selection.

## Security notes

- Do not commit `shared-services/stack.env`.
- Do not commit private cert/key files in `traefik/certs/`.
- Restrict direct DB/Redis host exposure if not needed.
