# Repository Description

* SANER.xlsx: Contains all the paper’s TABLEs
* CSVs: Contains all the sheets of SANER.xlsx as .csv
* sonarqube: Contains the necessary files in order to build the SonarQube Docker container
* sonarqube_conf: Contains the properties file for SonarQube
* docker-compose.yml: The .yml for docker-compose

# Replication Kit

## Requirements

* [Docker Community Edition](https://www.docker.com/community-edition#/download)
* [Docker Compose](https://docs.docker.com/compose/install/)


## Replication Instructions

### Clone the Repository
```bash
git clone https://github.com/td-evolution-in-apache/SANER2018
```

### Build the SonarQube Container
```bash
cd SANER2018/sonarqube
docker build --no-cache -t sonarqube:6.4 .
cd ..
```

### Restore the Database Dump
The execution of `wget` might take much time because size of sonar_dump.sql is 28.2 GB
```bash
wget 195.251.210.147/sonar_dump.sql
docker-compose rm -f; docker-compose up -d
```
Run `docker ps` in order to get the CONTAINER ID of the postgres container
```bash
cat sonar_dump.sql | docker exec -i POSTGRES_CONTAINER_ID psql -U sonar
```

### Add SonarQube Service to docker-compose File
```bash
docker-compose down
nano docker-compose.yml
```
And then, uncomment (delete #) the following commented-out lines and save the changes
```bash
#  sonarqube:
#    image: sonarqube:6.4
#    ports:
#      - 9000:9000
#    environment:
#      - TZ=Europe/Amsterdam
#      - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar
#    volumes:
#      - ./sonarqube_conf:/opt/sonarqube/conf
#      - ./sonarqube_data:/opt/sonarqube/data
```

### Start SonarQube
```bash
docker-compose up -d
```
