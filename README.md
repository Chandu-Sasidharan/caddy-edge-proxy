# Caddy Edge Proxy

Edge reverse proxy for projects hosted on the Hetzner server. It currently serves multiple domains and can be extended by adding more site blocks to the Caddyfile.

## What’s here

- Caddy 2.10.2 (alpine) via Docker Compose, exposed on ports 80/443.
- Automatic TLS via ACME; certs/config persisted in Docker volumes.
- External Docker network `edge_net` expected; backend/frontends join this network to be reachable.

## Prerequisites

- Docker + Docker Compose installed on the server.
- External network `edge_net` exists (`docker network create edge_net` if missing).
- Frontend/backend containers for each site attached to `edge_net` with matching service names.

## Adding another project

1. Ensure new app containers join edge_net and expose the ports you want Caddy to reverse proxy.
2. Add a site block to `Caddyfile` with the domain and `reverse_proxy` targets.
3. `docker compose exec caddy-edge caddy reload` to apply changes.
4. Confirm DNS points the domain to this server.

## Notes

- Volumes `caddy_data`/`caddy_config` persist TLS assets and config across restarts.
- Leave ports 80/443 free on the host; Caddy handles HTTP→HTTPS redirection.
