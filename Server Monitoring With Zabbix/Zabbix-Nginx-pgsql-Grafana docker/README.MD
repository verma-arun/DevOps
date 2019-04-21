> # This docker compose file contains Docker-compose files for running Zabbix with Postgre Sql for database, adminer for database management and Grafana for monitoring dashboard

## Zabbix-Server

Zabbix Server runs at localhost:80 
Default username: Admin
        password: zabbix

## Docker Images Used        

### PostgreSQL docker image
Runs via default port

#### Env Variables

- `POSTGRES_PASSWORD` - Password for user
- `POSTGRES_USER` - Set a user with username & password
- `PGDATA` - Volume for data storage (`/var/lib/postgresql/data` is default)
- `POSTGRES_DB` - Name of Database else POSTGRES_USER name will be used.
- `POSTGRES_INITDB_ARGS` - For sending arguments to Postgre
- `POSTGRES_INITDB_WALDIR` - For changing log directory



### Zabbix-Web-Nginx_Pgsql image

#### Env Variables

- `ZBX_SERVER_HOST` - This variable is IP or DNS name of Zabbix server. By default, value is zabbix-server.
- `ZBX_SERVER_PORT` - This variable is port Zabbix server listening on. By default, value is 10051.
- `DB_SERVER_HOST` - This variable is IP or DNS name of PostgreSQL server. By default, value is 'postgres-server'
- `DB_SERVER_PORT` - This variable is port of PostgreSQL server. By default, value is '5432'.
- `POSTGRES_USER`, `POSTGRES_PASSWORD` - These variables are used by Zabbix web interface to connect to Zabbix database. By default, values are zabbix, zabbix.
- `POSTGRES_DB` - The variable is Zabbix database name. By default, value is zabbix.
- `ZBX_HISTORYSTORAGEURL` - History storage HTTP[S] URL. This parameter is used for Elasticsearch setup. Available since 3.4.5.
- `ZBX_HISTORYSTORAGETYPES` - Array of value types to be sent to the history storage. An example: ['uint', 'dbl']. This parameter is used for Elasticsearch setup. Available since 3.4.5.
- `PHP_TZ` - The variable is timezone in PHP format. Full list of supported timezones are available on php.net. By default, value is 'Europe/Riga'.
- `ZBX_SERVER_NAME` - The variable is visible Zabbix installation name in right top corner of the web interface.
- `ZBX_MAXEXECUTIONTIME` - The varable is PHP max_execution_time option. By default, value is 300.
- `ZBX_MEMORYLIMIT` - The varable is PHP memory_limit option. By default, value is 128M.
- `ZBX_POSTMAXSIZE`- The varable is PHP post_max_size option. By default, value is 16M.
- `ZBX_UPLOADMAXFILESIZE` - The varable is PHP upload_max_filesize option. By default, value is 2M.
- `ZBX_MAXINPUTTIME`  The varable is PHP max_input_time option. By default, value is 300.



###  Zabbix-Server-Pgsql Image

#### Env Variables

- `DB_SERVER_HOST` - This variable is IP or DNS name of PostgreSQL server. By default, value is 'postgres-server'
- `DB_SERVER_PORT` - This variable is port of PostgreSQL server. By default, value is '5432'.
- `POSTGRES_USER`,`POSTGRES_PASSWORD` - These variables are used by Zabbix server to connect to Zabbix database. By default, values are zabbix, zabbix.
- `POSTGRES_DB` - The variable is Zabbix database name. By default, value is zabbix.
- `ZBX_LOADMODULE` - The variable is list of comma separated loadable Zabbix modules. It works with volume /var/lib/zabbix/modules. The syntax of the variable is dummy1.so,dummy2.so.
- `ZBX_DEBUGLEVEL` - The variable is used to specify debug level. By default, value is 3. It is DebugLevel parameter in zabbix_server.conf. Allowed values are listed below:
  - 0 - basic information about starting and stopping of Zabbix processes;
  - 1 - critical information
  - 2 - error information
  - 3 - warnings
  - 4 - for debugging (produces lots of information)
  - 5 - extended debugging (produces even more information)
- `ZBX_TIMEOUT` - The variable is used to specify timeout for processing checks. By default, value is 4.
- `ZBX_JAVAGATEWAY_ENABLE` - The variable enable communication with Zabbix Java Gateway to collect Java related checks. By default, value is false.

#### Some other variables

- `ZBX_LISTENIP=`
- `ZBX_HISTORYSTORAGEURL=` # Available since 3.4.0
- `ZBX_HISTORYSTORAGETYPES=uint,dbl,str,log,text` # Available since 3.4.0
- `ZBX_STARTPOLLERS=5`
- `ZBX_IPMIPOLLERS=0`
- `ZBX_STARTPREPROCESSORS=3` # Available since 3.4.0
- `ZBX_STARTPOLLERSUNREACHABLE=1`
- `ZBX_STARTTRAPPERS=5`
- `ZBX_STARTPINGERS=1`
- `ZBX_STARTDISCOVERERS=1`
- `ZBX_STARTHTTPPOLLERS=1`
- `ZBX_STARTTIMERS=1`
- `ZBX_STARTESCALATORS=1`
- `ZBX_STARTALERTERS=3` # Available since 3.4.0
- `ZBX_JAVAGATEWAY=zabbix-java-gateway`
- `ZBX_JAVAGATEWAYPORT=10052`
- `ZBX_STARTJAVAPOLLERS=5`
- `ZBX_STARTVMWARECOLLECTORS=0`
- `ZBX_VMWAREFREQUENCY=60`
- `ZBX_VMWAREPERFFREQUENCY=60`
- `ZBX_VMWARECACHESIZE=8M`
- `ZBX_VMWARETIMEOUT=10`
- `ZBX_ENABLE_SNMP_TRAPS=false`
- `ZBX_SOURCEIP=`
- `ZBX_HOUSEKEEPINGFREQUENCY=1`
- `ZBX_MAXHOUSEKEEPERDELETE=5000`
- `ZBX_SENDERFREQUENCY=30`
- `ZBX_CACHESIZE=8M`
- `ZBX_CACHEUPDATEFREQUENCY=60`
- `ZBX_STARTDBSYNCERS=4`
- `ZBX_HISTORYCACHESIZE=16M`
- `ZBX_HISTORYINDEXCACHESIZE=4M`
- `ZBX_TRENDCACHESIZE=4M`
- `ZBX_VALUECACHESIZE=8M`
- `ZBX_TRAPPERIMEOUT=300`
- `ZBX_UNREACHABLEPERIOD=45`
- `ZBX_UNAVAILABLEDELAY=60`
- `ZBX_UNREACHABLEDELAY=15`
- `ZBX_LOGSLOWQUERIES=3000`
- `ZBX_STARTPROXYPOLLERS=1`
- `ZBX_PROXYCONFIGFREQUENCY=3600`
- `ZBX_PROXYDATAFREQUENCY=1`
- `ZBX_TLSCAFILE=`
- `ZBX_TLSCRLFILE=`
- `ZBX_TLSCERTFILE=`
- `ZBX_TLSKEYFILE=`


##### Volumes for Storage 

- ` /usr/lib/zabbix/alertscripts ` - The volume is used for custom alert scripts. It is AlertScriptsPath parameter in zabbix_server.conf
- `/usr/lib/zabbix/externalscripts` - The volume is used by External checks (type of items). It is ExternalScripts parameter in zabbix server.conf.
- `/var/lib/zabbix/modules` - The volume allows load additional modules and extend Zabbix server using LoadModule feature.
- `/var/lib/zabbix/enc` - The volume is used to store TLS related files. These file names are specified using ZBX_TLSCAFILE, ZBX_TLSCRLFILE, ZBX_TLSKEY_FILE and ZBX_TLSPSKFILE variables.
- `/var/lib/zabbix/ssh_keys` - The volume is used as location of public and private keys for SSH checks and actions. It is SSHKeyLocation parameter in zabbix_server.conf.
- `/var/lib/zabbix/ssl/certs` - The volume is used as location of of SSL client certificate files for client authentication. It is SSLCertLocation parameter in zabbix_server.conf.
- `/var/lib/zabbix/ssl/keys` - The volume is used as location of SSL private key files for client authentication. It is SSLKeyLocation parameter in zabbix_server.conf.
- `/var/lib/zabbix/ssl/ssl_ca` - The volume is used as location of certificate authority (CA) files for SSL server certificate verification. It is SSLCALocation parameter in zabbix_server.conf.
- `/var/lib/zabbix/snmptraps` - The volume is used as location of snmptraps.log file. It could be shared by zabbix-snmptraps container and inherited using volumes_from Docker option while creating new instance of Zabbix server.
SNMP traps processing feature could be enabled using shared volume and switched ZBX_ENABLE_SNMP_TRAPS environment variable to true.
- `/var/lib/zabbix/mibs` - The volume allows to add new MIB files. It does not support subdirectories, all MIBs must be placed to /var/lib/zabbix/mibs.


### Grafana Image

Grafana provides dashboard for multiple applications
It runs on localhost:3030


### Adminer Image

Adminer (formerly phpMinAdmin) is a full-featured database management tool written in PHP. Conversely to phpMyAdmin, it consist of a single file ready to deploy to the target server. Adminer is available for MySQL, PostgreSQL, SQLite, MS SQL, Oracle, Firebird, SimpleDB, Elasticsearch and MongoDB.