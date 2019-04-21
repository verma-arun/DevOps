# Monitoring Servers with Zabbix 

## Introduction

Zabbix is an open source monitoring tool used for monitoring servers for resource usage like cpu load, memory usage , network and services running etc. We can add as much hosts on zabbix easily and monitor by using various means like:
- Zabbix agent - A small Daemon that runs on server/remote need to be monitored
- SNMP - A Zabbix server can collect data from devices with SNMP agent versions v1, v2 or v3. SNMP agents are present not only in network equipment, but also in printers, NAS, UPS. Basically, any equipment that is present on the network can be monitored through SNMP agents.
- IPMI Agent - To get important data from hardware, a Zabbix server supports IPMI agents, which are present by default on Intel architecture servers like HP iLO and Dell DRAC.

## Zabbix Options

### 1. Graph
Shows graph for cpu usage, memory usage, swap usage, cpu load, cpu utilization and more.

### 2. Triggers
Triggers are like callback to events happening, it is called when any event occurs like server is down or server is recovered. triggers are used to show information related on different conditions. We have triggers like memory usage is more, cpu usage is more, storage is less and more like these.

### 3. Screens 
Screens are a customizable panel option . We can add tables, graph, pie chart, bar chart as our need and requirement for different groups or different host like cpu monitor for one host , memory monitoring for another or vice versa.
We can arrange as our requirement and position different panels.

## Zabbix Tips/Use Cases/Solutions

### 1. Adding a new user
Go to Administration > users > create user.

### 2. Creating a User Group
User group helps to manage different user types like Administrators, monitoring persons, or more.
Go to Administration > User groups > Create User Group

## Docker Compose configuration for Zabbix and related services

```
version: '3.1'
services:
  postgres:
    image: postgres
    restart: always
    environment:
      POSTGRES_USER: zabbix
      POSTGRES_PASSWORD: zabbix
      POSTGRES_DB: zabbix
      PG_DATA: /var/lib/postgresql/data/pgdata
    ports:
      - 5434:5434
  zabbix-server:
    image: zabbix/zabbix-server-pgsql
    restart: always
    environment:
      DB_SERVER_HOST: postgres
      POSTGRES_USER: zabbix
      POSTGRES_PASSWORD: zabbix
      POSTGRES_DB: zabbix
      ZBX_HOUSEKEEPINGFREQUENCY: 1
      ZBX_MAXHOUSEKEEPERDELETE: 5000
    depends_on:
      - postgres
    volumes:
      - /usr/lib/zabbix/alertscripts
  zabbix-web:
    image: zabbix/zabbix-web-nginx-pgsql
    restart: always
    environment:
      ZBX_SERVER_HOST: zabbix-server
      ZBX_POSTMAXSIZE: 64M
      PHP_TZ: "Asia/Kolkata"
      DB_SERVER_HOST: postgres  
      POSTGRES_USER: zabbix
      POSTGRES_PASSWORD: zabbix
      POSTGRES_DB: zabbix
    depends_on:
      - postgres
      - zabbix-server
    ports:
      - 8090:80

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080

  grafana-xxl:
    image: monitoringartist/grafana-xxl:latest
    ports:
      - 3000:3000

```
List of Services used in *Docker-Compose* :

1. **Postgres** - For running PotgreSql as primary database for zabbix data store
. **Zabbix-Server** - For configuring connection to database and setting zabbix environment variables.

3. **Zabbix-Web** - The Frontend Zabbix UI in PHP along with Nginx Server

4. **Adminer(Optional)** - The Adminer is a UI for accessing database to view and perform queries from ui instead of terminal.

5. **Grafana-XXl(Optional)** - Grafana is an open source dashboard for monitoring from various data sources like zabbix, the onw we are using. We can create interactive panels, graphs , chart, pie charts in Grafana easily for monitoring different things of a server.

## Docker Compose configuration for Zabbix-Agent

```
version: '2'

services:
  dockbix-agent-xxl:
    image: monitoringartist/dockbix-agent-xxl-limited:latest
    privileged: true
    network_mode: "host"
    restart: on-failure
    volumes:
      - /:/rootfs
      - /var/run:/var/run
    environment:
      - ZA_Server=192.168.99.1,10.42.0.1

```
Here we have created Zabbix-agent service that runs on remote server we need to monitor. Zabbix agent runs on remote server and send data to Zabbix Server. 

## Steps for Running Zabbix via above Docker-compose configuration

1. Open Directory with docker-compose.yml with the same configuration above in terminal.

2. Run command :  `sudo docker-compose up -d` in terminal.

3. Now open browser and go to address  `localhost:8090` for accessing Zabbix Web UI.

4. Enter Default Username & Password for Zabbix 
    ```
    Username Admin
    Password zabbix 
    ```
5. Run The same way docker-compose in VM with zabbix-agent compose file.

6. Now go to Configuration > Hosts and create New Host.

7. Fill the fields in the form , select an already created group like *Virtual machines* if you are using *VM* or create a new group like *Custom Group*.

8. Add description and ip address with port for services. If you are using default ports then only add ip address of server you need to monitor like `192.168.99.100` with port `10050` for zabbix-agent based monitoring.

9. After saving , click on newly created host name , then templates > add new and search template with name *Template Linux Os* for setting default configuration for monitoring applications, services, disk space and cpu usage.

9. Now Monitoring can be done on that host.

