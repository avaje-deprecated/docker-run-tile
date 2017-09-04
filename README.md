# docker-run-tile
Maven tile for the docker-run-maven-plugin (to start/stop containers as part of maven build lifecycle)

## WHAT IT DOES

#### Starts containers (before tests run)

- Starts containers prior to tests running on the `process-test-classes` phase
- Starts containers like Postgres, MySql, ElasticSearch, Redis 
- For DB containers it can drop and create user, database and extensions etc


#### Stop containers (after tests run)

- Stop containers when tests have completed on the `prepare-package` phase
- Note we generally do not stop the containers running `mvn clean test` (frequent developer use)
- Note we generally DO stop and remove containers on `mvn clean package` or `mvn clean install` (usual for build agent use)


## How to use

#### 1. Add docker-run.properties

Add docker-run.properties to src/test/resources

This defines the configuration for which containers to start and stop as part of the build. We can start DB's like Postgres but could also manage containers for ElasticSearch, Redis etc. 

#### 2. Add tile to pom.xml

```xml
<plugin>
  <groupId>io.repaint.maven</groupId>
  <artifactId>tiles-maven-plugin</artifactId>
  <version>2.10</version>
  <extensions>true</extensions>
  <configuration>
    <filtering>false</filtering>
    <tiles>
      <tile>org.avaje.tile:docker-run:0.1</tile>
    </tiles>
  </configuration>
</plugin>

```


### Example docker-run.properties

```properties

# start postgres container (defaults to port 6432)
dbPlatform=postgres

# define db name, user etc 
dbName=unit_db_foo
dbUser=unit_db_user
dbPassword=unit_db_password

# for Postgres we can install extensions
dbExtensions=hstore,pgcrypto

```
