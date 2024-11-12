# QuantumFlow with Elasticsearch and Kibana
A Docker Compose setup for deploying QuantumFlow with Elasticsearch and Kibana.

![how it works docker container version](https://github.com/user-attachments/assets/39079e14-6745-4135-9fc5-b1fb5dab825c)

## Requirements
- [**Docker** and **Docker Compose** installed on your machine.](https://docs.docker.com/engine/install/)
    - [Install Docker on Ubuntu Linux](https://docs.docker.com/engine/install/ubuntu/)
    - [Install Docker on Debian Linux](https://docs.docker.com/engine/install/debian/)
    - [Install Docker on Redhat Linux](https://docs.docker.com/engine/install/rhel/)
- [**You've cloned this Git Repo**]

## Optional Requirements
- **GeoLite2 Database** (Optional, required for enhanced geolocation capabilities). Update /etc/quantumflow/maxmind/ (asn.mmdb & city.mmdb) by registering at maxmind.com.
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
Open a browser and go to [http://localhost:5601](http://localhost:5601) (From local machine) or http://ip-of-host:5601 to access Kibana.
![DASH1](https://github.com/user-attachments/assets/e3587823-1a2e-4839-976c-5214629ddc93)
![dash example](https://github.com/user-attachments/assets/9c26248f-13d1-4f80-82f6-d4feae1a954b)
## Stopping the Services
To stop all containers, run:

```bash
docker-compose down
