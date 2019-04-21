# Minio Single Node

##### Running a minio service on single host.
#
## Minio docker-compose.yml File

##### Define the services that run an minio image in docker-compose.yml. so they can be run together in an isolated environment.

#
#
#

#### Define Service
            services:
                minio1:
                    image: minio/minio
#### Define Named Volumes            
            volumes:
               - minio_data1:/data

#### Expose Port and Map To Container
            ports:
              - 9001:9001

#### Setting Environment Variable ( Minio Secret Key and Password )
            environment:
              - MINIO_ACCESS_KEY=miniokey
              - MINIO_SECRET_KEY=miniosecret
            command: server /data

### Define Volume Options
##### Driver for local or host storage, Options for type volume or mount and absolute path
#
            volumes:
                minio_data1:
                  driver: local
                  driver_opts:
                    o: bind
                    type: none
                    device: /var/new_db/minio/minioone/

## Run Service
        docker-compose -f docker-compose.yml up -d
