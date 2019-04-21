# Zabbix-Agent Docker image is used to run Zabbix agent in Docker Compose

It runs on Server to be monitored by consuming only 5 mb approx memory and very low cpu cycle less than 1%

```
docker run --name some-zabbix-agent -e ZBX_HOSTNAME="some-hostname" -e ZBX_SERVER_HOST="some-zabbix-server" -d zabbix/zabbix-agent:tag
```
Where some-zabbix-agent is the name you want to assign to your container, some-hostname is the hostname, it is Hostname parameter in Zabbix agent configuration file, some-zabbix-server is IP or DNS name of Zabbix server or proxy and tag is the tag specifying the version you want.