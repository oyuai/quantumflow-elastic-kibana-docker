# QuantumFlow with Elasticsearch and Kibana
A Docker Compose setup for deploying QuantumFlow with Elasticsearch and Kibana.

![img](https://oyu.ai/wp-content/uploads/2024/11/how-it-works-docker-container-version-scaled.jpg)

## Requirements
- [**Docker** and **Docker Compose** installed on your machine.](https://docs.docker.com/engine/install/)
  - [Install Docker on Ubuntu Linux](https://docs.docker.com/engine/install/ubuntu/)
  - [Install Docker on Debian Linux](https://docs.docker.com/engine/install/debian/)
  - [Install Docker on Redhat Linux](https://docs.docker.com/engine/install/rhel/)

## Optional Requirements
- **GeoLite2 Database** (Optional, required for enhanced geolocation capabilities).
  - Register at [MaxMind](https://www.maxmind.com) to get a license key and download GeoLite2 data to update maxmind files with current mmdb data.
  
## Setup **(DON'T FORGET TO UPDATE APP.CONFIG.YML WITH YOUR LICENSE KEY)**

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/oyuai/quantumflow-elastic-kibana-docker.git
   cd quantumflow-elastic-kibana-docker
2. **Edit the License in the `app.config.yml` File**:

   To add your license, open the `app.config.yml` file in a text editor, such as `vi`:

   ```bash
   # Open the configuration file in vi to edit the license field
   vi ./etc/quantumflow/app.config.yml
   ```
   
   Inside the file, locate the `license` field and replace `INSERT-LICENSE-HERE` with your actual license key:
   ```yaml
   license: "INSERT-YOUR-LICENSE-KEY-HERE"
   ```
## Run Docker Compose:

```bash
docker-compose up -d
```


## Access Kibana:
Open a browser and go to [http://localhost:5601](http://localhost:5601) (if navigating directly from machine running docker container) or http://ip-of-host:5601 to start using Kibana.

## Stopping the Services
To stop all containers, run:

```bash
docker-compose down
