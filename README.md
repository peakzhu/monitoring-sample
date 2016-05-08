# Custom monitoring sample for Aliyun Container Service

### Build the images

```
./build.sh
```

### Play with the Demo

For InlfuxDB 0.8.8, using the following docker-compose template

```
influxsrv:
  image: registry.aliyuncs.com/acs-sample/tutum-influxdb:0.8.8
  ports:
    - "8083:8083"
    - "8086:8086"
  expose:
    - "8090"
    - "8099"
  environment:
    - PRE_CREATE_DB=cadvisor
cadvisor:
  image: registry.aliyuncs.com/acs-sample/google-cadvisor:v0.20.1
  command: -storage_driver=influxdb -storage_driver_db=cadvisor -storage_driver_host=influxsrv:8086
  ports:
    - "9090:8080"
  volumes:
    - /:/rootfs:ro
    - /var/run:/var/run:rw
    - /sys:/sys:ro
    - /var/lib/docker/:/var/lib/docker:ro
  links:
    - influxsrv:influxsrv
  labels:
    aliyun.global: "true"
grafana:
  image: registry.aliyuncs.com/acs-sample/grafana:2.0
  ports:
    - "3000:3000"
  links:
    - influxsrv:influxsrv
  environment:
    - INFLUXDB_HOST=influxsrv
    - INFLUXDB_PORT=8086
    - INFLUXDB_NAME=cadvisor
    - INFLUXDB_USER=root
    - INFLUXDB_PASS=root
  labels:
    aliyun.routing.port_3000: 'http://grafana'
config:
  image: registry.aliyuncs.com/acs-sample/grafana-config:0.8
  links:
    - influxsrv:influxsrv
    - grafana:grafana

```

For InlfuxDB 0.9, using the following docker-compose template

```
influxsrv:
  image: registry.aliyuncs.com/acs-sample/tutum-influxdb:0.9
  ports:
    - "8083:8083"
    - "8086:8086"
  expose:
    - "8090"
    - "8099"
  environment:
    - PRE_CREATE_DB=cadvisor
cadvisor:
  image: registry.aliyuncs.com/acs-sample/google-cadvisor:v0.20.5
  command: -storage_driver=influxdb -storage_driver_db=cadvisor -storage_driver_host=influxsrv:8086
  ports:
    - "9090:8080"
  volumes:
    - /:/rootfs:ro
    - /var/run:/var/run:rw
    - /sys:/sys:ro
    - /var/lib/docker/:/var/lib/docker:ro
  links:
    - influxsrv:influxsrv
  labels:
    aliyun.global: "true"
grafana:
  image: registry.aliyuncs.com/acs-sample/grafana:2.6
  ports:
    - "3000:3000"
  links:
    - influxsrv:influxsrv
  environment:
    - INFLUXDB_HOST=influxsrv
    - INFLUXDB_PORT=8086
    - INFLUXDB_NAME=cadvisor
    - INFLUXDB_USER=root
    - INFLUXDB_PASS=root
  labels:
    aliyun.routing.port_3000: 'http://grafana'
config:
  image: registry.aliyuncs.com/acs-sample/grafana-config:0.9
  links:
    - influxsrv:influxsrv
    - grafana:grafana

```

Launch the monitoring application with following command

```
docker-compose up -d
```

-------------------
* Li Yi (denverdino@gmail.com)