# Grafana and InfluxDB

An optimized Docker version of Grafana and InfluxDB as time series database.  
All persistent volumes are already set and they are mounted at containers run.  
You could edit configuration file of all services, a Nginx reverse proxy extend web server functionalities like TLS connections in production environments.  

## Quickstart 

Start in less than 5 minutes.

## 1 - Environment File
Create a new environment variables `.env` file in project root folder and set respective value as shown below:

```
GRAFANA_EXT_PORT=3000
GRAFANA_SERVER_ROOT_URL=http://localhost
GRAFANA_SECURITY_ADMIN_PASSWORD=admin
INFLUXDB_EXT_PORT=3306
INFLUXDB_DB=grafana
INFLUXDB_HTTP_AUTH_ENABLED=true
INFLUXDB_ADMIN_USER=grafana
INFLUXDB_ADMIN_PASSWORD=grafana
NGINX_EXT_PORT=80
```

**2 - Create configuration files**  
Clone following files w/o `-sample` suffix in the file name:  

- `grafana/defaults-sample.ini`
- `influxdb/influxdb-sample.conf`
- `nginx/default-sample.conf`

CLI Commands

```
cp grafana/defaults-sample.ini grafana/defaults.ini
cp influxdb/influxdb-sample.conf influxdb/influxdb.conf
cp nginx/default-sample.conf nginx/default.conf 
```

Feel free to change configuration files with your custom values.

**3 - Run containers through Docker Compose**  

Start Grafana and InfluxDB Docker containers:

```
# Load environment variable
source .env

# Run containers
docker-compose up -d
```

Stop containers
```
docker-compose down
```

**4 - Connect to Grafana UI**  
Run in your browser Grafana URL and login into the UI

```
# Local environment
http://localhost

# Online instance
http://your.domain.com
```

## Containers Stack
The service is composed by three containers:

- **Grafana**: Charts and metrics UI
- **InfluxDB**: Time series database to store all metrics
- **Nginx**: Reverse proxy to manage HTTP/S requests and proxy to Grafana
 

### Grafana

Current version: `latest` on [Docker Hub](https://hub.docker.com/r/grafana/grafana)

You can log in to Grafana UI as `admin` user and password set in `.env` file.

### InfluxDB

Current version: **1.7**

Test connection to InfluxDB

```
# Run a container as InfluxDB client and send a request to InfluxDB container by curl
GRAFANA_INFLUXDB_NET=`docker network ls | grep grafana-influx | tr -s ' ' | cut -d ' ' -f 2`
docker run --rm -t \
    --network=$GRAFANA_INFLUXDB_NET \
    influxdb:1.5.4 \
    curl -G http://influxdb:8086/query -u db_user:db_passwd --data-urlencode "q=SHOW DATABASES"
```

### Nginx

Current version: **1.19**

Default configuration allows Nginx to forward request to Grafana port (3000).  
Improve Nginx configuration based on your environment requirements.  

## Persistent Storage

Local folders and Docker volumes are mapped in order to keep important data stored in host storage.  
In case you will destroy and re-run containers stack any data will be lost.

For more information about mounted volumes check `docker-compose.yml`.

