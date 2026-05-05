# Traefik configuration

This folder contains Traefik files used by the file provider.

## Structure

- `dynamic/services.yaml`: HTTP routers, services, and middlewares.
- `dynamic/tls.yaml`: TLS certificates and default certificate store.
- `certs/`: local certificate and key files (ignored by git).

## Expected cert files

The current `dynamic/tls.yaml` expects these files inside container:

- `/etc/traefik/certs/dev.sby.test.crt`
- `/etc/traefik/certs/dev.sby.test.key`

If you mount `~/projects/surabaya-dev/traefik/certs:/etc/traefik/certs`, place these files in:

- `traefik/certs/dev.sby.test.crt`
- `traefik/certs/dev.sby.test.key`

## Apply changes

1. Edit files in `dynamic/`.
2. Redeploy Traefik if needed (file provider watch is enabled, so many changes are hot-reloaded).
3. Verify with `https://traefik.dev.sby.test` dashboard and target hostnames.
