# QuantumFlow — Docker Compose Deployment

Run the full QuantumFlow flow-monitoring stack (Elasticsearch, Kibana, dedupe, QuantumFlow, and Layer8) with Docker Compose.

---

## Prerequisites

- A host with [Docker Engine](https://docs.docker.com/engine/install/) and the Docker Compose plugin (`docker compose`).
- At least **8 GB RAM** free (Elasticsearch alone is configured for a 2 GB JVM heap by default).
- A valid **OyuAI license token**.
- Outbound internet access to pull images from `docker.elastic.co` and Docker Hub (`oyuai/*`).

---

## 1. Create your `.env` file

Compose reads configuration from a file named `.env` in the same directory as `docker-compose.yml`. A template named `env` is provided in `quantumflow/`. Copy it to `.env`:

```bash
cd quantumflow
cp env .env
```

> On Windows PowerShell: `Copy-Item env .env`

The `.env` template looks like this:

```bash
# Elastic stack version
ELASTIC_VERSION=9.1.4

# OyuAI image tags
QUANTUMFLOW_TAG=latest
DEDUPE_TAG=latest
LAYER8_TAG=latest

# Elasticsearch JVM heap (tune to available memory)
ES_JAVA_OPTS=-Xms2g -Xmx2g

# Elasticsearch security (HTTP, no TLS in this stack)
ELASTIC_BOOTSTRAP_PASSWORD=changeme   # built-in elastic superuser (bootstrap only)
KIBANA_SYSTEM_PASSWORD=changeme       # Kibana server -> Elasticsearch backend
ELASTIC_USER=oyuai                    # login used for the Kibana UI
ELASTIC_PASSWORD=elastic

# REQUIRED:
LICENSE_TOKEN=<your-license-token-here>
```

---

## 2. Add your license

The only **required** change is the license. Set `LICENSE_TOKEN` in your `.env` to the token provided by OyuAI:

```bash
LICENSE_TOKEN=eyJhbGciOiJFZERTQSIsInR5cCI6IkpXVCJ9....
```

You should also change the default passwords (`changeme`, `elastic`) before exposing this stack on a network.

---

## 3. Start the stack

From the `quantumflow/` directory (where `docker-compose.yml` and `.env` live):

```bash
docker compose up -d
```

This pulls the images and starts every service in the background. The first launch takes a few minutes while Elasticsearch initializes and `es-setup` configures the user accounts.

Check progress with:

```bash
docker compose ps
docker compose logs -f
```

Wait until `elasticsearch` and `kibana` report `healthy`.

---

## 4. Access the services

| Service | URL / Port | Notes |
| --- | --- | --- |
| **Kibana** | http://localhost:5601 | Web UI — log in as `oyuai` / `elastic` (the `ELASTIC_USER` / `ELASTIC_PASSWORD` values) |
| **Elasticsearch** | http://localhost:9200 | REST API (HTTP, no TLS) |
| QuantumFlow — NetFlow v9 | `2055/udp` | Flow collector |
| QuantumFlow — IPFIX | `4739/udp` | Flow collector |
| QuantumFlow — sFlow | `6343/udp` | Flow collector |
| QuantumFlow — Netflow/IPFIX (alt) | `4740/udp` | Flow collector |
| QuantumFlow — API | `8081/tcp`, `8443/tcp` | HTTP / HTTPS |

Point your network devices' flow exporters at the host IP on the relevant UDP ports above.

---

## Services overview

| Service | Image | Purpose |
| --- | --- | --- |
| `elasticsearch` | `docker.elastic.co/elasticsearch/elasticsearch` | Data store (single-node) |
| `es-setup` | (same as above) | One-shot job that sets the `kibana_system` and `oyuai` passwords, then exits |
| `kibana` | `docker.elastic.co/kibana/kibana` | Dashboards and UI |
| `dedupe` | `oyuai/dedupe` | Flow de-duplication |
| `quantumflow` | `oyuai/quantumflow` | Flow ingestion / collector |
| `layer8` | `oyuai/layer8` | Enrichment / analytics |

Persistent data is stored in the `es-data` and `kibana-data` Docker volumes. Per-service config lives under `etc/<service>/app.config.yml`.

---

## Common commands

```bash
# Stop the stack (keeps data volumes)
docker compose down

# Stop and DELETE all data volumes
docker compose down -v

# Restart a single service
docker compose restart quantumflow

# Tail logs for one service
docker compose logs -f kibana
```

---

## Troubleshooting

- **Kibana not loading?** It depends on Elasticsearch being healthy and on `es-setup` finishing successfully. Run `docker compose logs es-setup` to confirm the user passwords were set.
- **Elasticsearch exits / OOM:** lower or raise `ES_JAVA_OPTS` in `.env` to match your host RAM, then `docker compose up -d` again.
- **`vm.max_map_count` errors (Linux):** run `sudo sysctl -w vm.max_map_count=262144`.
- **License errors on `quantumflow` / `dedupe` / `layer8`:** verify `LICENSE_TOKEN` is set correctly in `.env`, then recreate the containers with `docker compose up -d`.
