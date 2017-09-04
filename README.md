# docker-run-tile
Maven tile for the docker-run-maven-plugin (to start/stop containers as part of maven build lifecycle)

## Start containers

- Start containers prior to tests running (on the `process-test-classes` phase)
- Start containers like Postgres, ElasticSearch, Redis at this point
- For DB containers it can drop and create user, database and extensions 


## Stop containers

- Stop containers when tests have completed on the `prepare-package` phase
- Note we generally do not stop the containers running `mvn clean test` (frequent developer use)
- Note we generally DO stop and remove containers on `mvn clean package` or `mvn clean install` (usual for build agent use)


## Add docker-run.properties

Add docker-run.properties to src/test/resources

This defines the configuration for which containers to start and stop as part of the build.

## Add tile to pom.xml

```xml
<plugin>
	<groupId>io.repaint.maven</groupId>
	<artifactId>tiles-maven-plugin</artifactId>
	<version>2.8</version>
	<extensions>true</extensions>
	<configuration>
		<filtering>false</filtering>
		<tiles>
			<tile>org.avaje.tile:docker-run:0.1</tile>
		</tiles>
	</configuration>
</plugin>

```


## Example docker-run.properties

```properties
dbPlatform=postgres
dbName=unit_features
dbUser=unit_features
dbPassword=unit_features
dbExtensions=hstore,pgcrypto
#dbStartMode=dropCreate

```
