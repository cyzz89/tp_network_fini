# TP docker

## Ce qui tourne
 Traefik (80/443, redirection http vers https) : app.localhost et monitoring.localhost Grafana (3000).
 App Flask + MariaDB (pas exposés).
 cAdvisor : http://localhost:6789.
 Grafana : admin monitoring/monitoring, derrière Traefik (plus de port direct).
 Prometheus :  http://localhost:9090.

## TLS / Traefik
 Certs montés depuis ./certs (app.localhost.crt/key). Sinon Traefik sert de l’auto-signé.
 Hosts à ajouter : 127.0.0.1 app.localhost et monitoring.localhost.
 Entrypoint web redirige vers websecure.

## Ports occupés
 8080 occupé (dashboard Traefik) remappé.
 1406 occupé Grafana derrière Traefik.
 cAdvisor mis sur 6789 car manque de place.

## État Grafana et Prometheus
 Dashboard Grafana créé mais données incomplètes (CPU/RAM/Disk)
 Prometheus données cAdvisor toutes les 15s, données non completes
## Fichiers à connaître
 compose.yml : services, labels Traefik, ports, volumes certs.
 traefik/traefik.yaml : entrypoints, redirection HTTP→HTTPS.
 prometheus/prometheus.yml : job cAdvisor.

## À finir
 Compléter les données et stabiliser le dashboard.



