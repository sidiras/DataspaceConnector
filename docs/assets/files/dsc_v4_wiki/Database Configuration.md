The Dataspace Connector uses Spring Data JPA to set up the database and manage interactions with it. 
Spring Data JPA supports many well-known relational databases out of the box. Thus, the internal H2 
can be replaced by e.g. MySQL, PostgreSQL, or Oracle databases with minimal effort.

## Using different databases

To use another database for the Connector, follow these steps:

1. Add the dependency for your chosen database to the Connector's `pom.xml` (contains required JDBC driver).
2. Adjust the following parameters in `src/main/resources/application.properties`:

    ```
    * spring.datasource.url
    * spring.datasource.platform
    * spring.datasource.driver-class-name
    * spring.datasource.username
    * spring.datasource.password
    * spring.jpa.database-platform
    ```

3. Check what types for large objects your chosen database supports. In some entity classes, some fields 
reference another object and the column definition for the respective fields is set to the value `BLOB` 
(`@Column(columnDefinition = "BLOB")`). This type works for H2 but might not be supported in your chosen 
database. In that case, set the column definition to a supported value. The following classes contain this column definition:

    ```
    * de.fraunhofer.isst.dataspaceconnector.model.OfferedResource
    * de.fraunhofer.isst.dataspaceconnector.model.RequestedResource
    * de.fraunhofer.isst.dataspaceconnector.model.ResourceMetadata
    ```

Spring will take care of the rest. The next time you start the Connector, it will use the newly 
configured database. If the tables have not been created in the new database, remember to set the 
property `spring.jpa.hibernate.ddl-auto` to the value `create` for the first application start. 
Spring will generate tables for all entities. For consecutive application starts, the property can be 
set to `update`, if data should not be lost.	

Keep in mind that when using an external database, the specified database is required to be running 
and reachable when starting the Connector, otherwise it will fail. So you will either have to run a 
database instance locally or, in case of using Docker, add a database container to your setup. Docker 
images of common databases are freely available.


## Example

Hereinafter, an example on how to use a PostgreSQL database with the Connector will be given. 

### Setup PostgreSQL 

The following steps are the same for other databases, just with different values.

1. Add PostgreSQL dependency to the `pom.xml`:

    ```
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.2.5</version>
    </dependency>
    ```

2. Adjust datasource properties in `application.properties`:

    ```
    spring.datasource.url = jdbc:postgresql://[postgres-host]:5432/[database-name]
    spring.datasource.platform = postgres
    spring.datasource.driver-class-name = org.postgresql.Driver
    spring.datasource.username = [username]
    spring.datasource.password = [password]
    spring.jpa.database-platform = org.hibernate.dialect.PostgreSQLDialect
    ```

3. Change column definition from `BLOB` to `BYTEA`, which is supported in Postgres:

    ```
    @Column(columnDefinition = "BYTEA")
    ```
	
#### Execute Docker	
	
To run the connector with docker-compose, a PostgreSQL container has to be added to the setup. 
Therefore, add the following service to the `docker-compose.yaml`:

    postgres:
      image: postgres
      ports:
        - "5432:5432"
      env_file:
        - ./postgres.env
	  
In the file `postgres-params.env`, specify the parameters database name, username, and password for 
the PostgreSQL container:

    POSTGRES_USER=connector
    POSTGRES_PASSWORD=12345
    POSTGRES_DB=connectordb
	
The last step is to set the Connector's properties so that it uses the database provided in the 
container. To achieve this, add an env file to the Connector service in `docker-compose.yaml`:

    env_file:
      - ./connector.env
	  
In this file, specify the following parameters:

    SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/connectordb
    SPRING_DATASOURCE_USERNAME=connector
    SPRING_DATASOURCE_PASSWORD=12345

To ensure that the Connector container waits until the Postgres container is ready, before it tries to connect, also add this to the Connector service in ``docker-compose.yaml``:

    depends_on:
      - postgres

By default, all data in the Postgres container will be lost when running `docker-compose down`. 
If you want to keep your data persisted across restarts, add a persistent volume in the `docker-compose.yaml`:

    services:
      postgres:
        image: postgres
        volumes:
          - connector-data:/var/lib/postgresql/data


    volumes:
      connector-data: {}
