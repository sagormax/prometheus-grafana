# prometheus-grafana
log monitoring

## Docker compose file
``` yml
services:
  node_exporter:
    image: quay.io/prometheus/node-exporter:latest
    container_name: node_exporter
    command:
      - '--path.rootfs=/host'
    network_mode: host
    pid: host
    restart: unless-stopped
    volumes:
      - '/:/host:ro,rslave'

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - 9090:9090
    restart: unless-stopped
    volumes:
      - ./web.yml:/etc/prometheus/web.yml
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--web.config.file=/etc/prometheus/web.yml'

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - 3000:3000
    restart: unless-stopped
    volumes:
      - grafana-data:/var/lib/grafana

volumes:
  grafana-data:
    driver: local
  prometheus-data:
    driver: local
```