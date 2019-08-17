# Grafana and InfluxDB

An optimized docker version of Grafana with InfluxDB as data source.  
All persistent volumes are already set and they are mounted at container run.  
You could edit configuration file of all services, a Nginx reverse proxy extend web server functionalities like TLS connections in production environments.  

## Quickstart 

**1 - Environment File**  
Create a new environment variables `.env` file in project root folder and set respective value as shown below:

```
GF_SERVER_ROOT_URL=http://your.domain.com
GF_SECURITY_ADMIN_PASSWORD=mypassword

INFLUXDB_DB=db_name
INFLUXDB_HTTP_AUTH_ENABLED=true
INFLUXDB_ADMIN_USER=db_user
INFLUXDB_ADMIN_PASSWORD=db_passwd

```

**2 - Create Grafana configuration file**  
Copy and paste sample file defaults-sample.ini in the same folder.

```
cp grafana/defaults-sample.ini grafana/defaults.ini
```

Run Docker containers by Docker Compose
```
docker-compose up -d
```

Stop containers
```
docker-compose down
```

Connect to Grafana UI

```
# Local environment
http://localhost

# Online instance
http://your.domain.com
```

## Grafana

You can apply your custom configuration in `grafana/defaults.ini` file


## InfluxDB

Test connection to InfluxDB

```
# Run a container as InfluxDB client and send a request to curl
GRAFANA_INFLUXDB_NET=`docker network ls | grep grafana-influxdb | tr -s ' ' | cut -d ' ' -f 2`
docker run --rm -t \
    --network=$GRAFANA_INFLUXDB_NET \
    influxdb:1.5.4 \
    curl -G http://influxdb:8086/query -u db_user:db_passwd --data-urlencode "q=SHOW DATABASES"
```