# TP Réseau / Observabilité – résumé humain

## Ce qui tourne
- Traefik (80/443, redir HTTP→HTTPS) : `app.localhost` → app (5000), `monitoring.localhost` → Grafana (3000).
- App Flask + MariaDB (pas exposés).
- cAdvisor : métriques conteneurs sur `http://localhost:6789`.
- Grafana : admin `monitoring/monitoring`, derrière Traefik (plus de port direct).
- Prometheus : scrape cAdvisor, UI sur `http://localhost:9090`.

## TLS / Traefik
- Certs montés depuis `./certs` (`app.localhost.crt/key`). Sinon Traefik sert de l’auto-signé.
- Hosts à ajouter : `127.0.0.1 app.localhost` et `monitoring.localhost`.
- Entrypoint `web` redirige vers `websecure`.

## Ports galère rencontrés
- 8080 occupé (dashboard Traefik) → remappé.
- 1406 occupé → Grafana basculé derrière Traefik.
- cAdvisor mis sur 6789 faute de place.

## État Grafana / Prometheus
- Dashboard Grafana créé mais métriques incomplètes (CPU/RAM/Disk I/O/Uptime par conteneur pas toutes ok).
- Prometheus scrape cAdvisor toutes les 15s ; données partielles à raffiner.

## Fichiers à connaître
- `compose.yml` : services, labels Traefik, ports, volumes certs.
- `traefik/traefik.yaml` : entrypoints, redir HTTP→HTTPS.
- `prometheus/prometheus.yml` : job cAdvisor.

## À finir
- Compléter les métriques et stabiliser le dashboard, puis l’exporter en JSON.
- Clarifier/déployer les certs custom si besoin.


