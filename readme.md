
# Sonarqube 1.7 with Branch 2.0.0 plugin

*This repository contains resources to run Sonarqube 1.7 in a docker container, using PostrgreSQL as database on a dependant container. Both container will make use of VOLUMEs for persistency.*
*Sonarqube image comes with the Branch 2.0.0 plugin that allows changing the name of the single branch that can be ananlyzed with this free version*

## Build local .dockerfile 
Open the *recepies/docker-compose.yml* file and make shure that you have the following configuration:

```
services:
  sonarqube:
    #image: eux86/sonarqube:v17plusbranch200
    build: ../sonarqube17plusBranch200/.
    image: sonarqube17branch200
    [...]
```

## Pull image from Docker Hub
Open the *recepies/docker-compose.yml* file and make shure that you have the following configuration:

```
services:
  sonarqube:
    image: eux86/sonarqube:v17plusbranch200
    #build: ../sonarqube17plusBranch200/.
    #image: sonarqube17branch200
    [...]
```

## How to run Sonarqube with persistency
The docker-compose.yml file, unde the folder *recepies*, manages how the configuration needed to run the sonarqube and the postgreSQL containers and connect them under the same network, also creating volumes for persistency.

To run the system, simply open your command shell and *cd* into the *recepies* folder. Then copy and paste this command:

```bash
docker-compose up
``` 

Add the -d parameter to detach the process and free the command shell:
```bash
docker-compose up -d
``` 

## Run a temporary image of Sonarqube 
### Note: this method does not allow persistency

Open your command shell and *cd* into the *sonarqube17plusBranch200* folder. Then copy and paste this command:

```bash
docker build -t sonarqube .
docker run --name sonarqube -p 9000:9000 --rm sonarqube
``` 

## Sonarqube default credentials:
* **Username**: admin
* **Password**: admin


## Reset images and volumes
**WARNING** this command will **PERMANENTLY REMOVE** all the history and configurations.

```bash
docker image rmi sonarqube17branch200
docker image rmi postgres
docker volume rm dockersonarqube_postgresql
docker volume rm dockersonarqube_postgresql_data
docker volume rm dockersonarqube_sonarqube_conf
docker volume rm dockersonarqube_sonarqube_data
docker volume rm dockersonarqube_sonarqube_extensions
``` 

