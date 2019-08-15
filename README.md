# Grafana and InfluxDB

An optimized docker version of Grafana with InfluxDB as data source.  
All persistent volumes are already set and they are mounted at container run.  
You could edit configuration file of all services, a Nginx reverse proxy extend web server functionalities like TLS connections in production environments.  

## Quick start 

Create a new environment variables file (*.env*) in project root folder 

```
GF_SERVER_ROOT_URL: "http://your.domain.com"
GF_SECURITY_ADMIN_PASSWORD: "ADMIN_PASSWORD"
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
## 