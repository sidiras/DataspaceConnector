The Dataspace Connector provides multiple ways for logging and accessing information.

## Configuration

### Static Configuration
To configure the logging find `log4j2.xml` at `src/main/resources`.
There, you will find the different loggers and the target outputs used within the Dataspace Connector.

To change the logging level of the Dataspace Connector modify the attribute `level` of the logger 
named `de.fraunhofer.isst.dataspaceconnector`. The different values of logging level can be found 
[here](https://logging.apache.org/log4j/2.x/manual/configuration.html#SystemProperties).

```xml
<Logger name="de.fraunhofer.isst.dataspaceconnector" level="info">
    <AppenderRef ref="ConsoleAppender"/>
</Logger>
```

The `AppenderRef` of the Logger controls the output of the log. Add or remove elements of type 
`AppenderRef` to add additional outputs or remove existing ones.

The Dataspace Connector offers preconfigured appenders. For logging to console use `ConsoleAppender` 
or to file use `RollingFile` as values for the `ref` attribute.

```xml
<Logger name="de.fraunhofer.isst.dataspaceconnector" level="debug">
    <AppenderRef ref="ConsoleAppender"/>
    <AppenderRef ref="RollingFile"/>
</Logger>
```

Note that the root logger is already logging to file by default. This logger already contains all 
logs of the `de.fraunhofer.isst.dataspaceconnector` logger.

To add additional logging outputs or change the logging format consult 
[here](https://logging.apache.org/log4j/2.x/manual/appenders.html) or for more information 
see [here](https://logging.apache.org/log4j/2.x/manual/configuration.html#XML).

### Runtime Configuration
The Dataspace connector allows the modification of logging levels at runtime. To enable this feature you 
will need to locate `application.properties` under `src/main/resources`. 

Enable or add the following lines:
```
management.endpoints.web.exposure.include=loggers
management.endpoint.loggers.enabled=true
```

A list of all available loggers and their current logging level will be exposed under `/actuator/loggers`.

To change the logging level at runtime you will need to perform a `POST` request against the logger.
Here is an example using curl:
```
curl -i -k -X POST -H 'Content-Type: application/json' -d '{"configuredLevel": "OFF"}' https://localhost:8080/actuator/loggers/de.fraunhofer.isst.dataspaceconnector
```

## Remote Access
To get remote access to the log file find the `application.properties` at `src/main/resources`.
By default, the Dataspace Connector disables all optional endpoints.

Enable or add the following lines:
```
management.endpoints.web.exposure.include=logfile
management.endpoint.logfile.enabled=true
management.endpoint.logfile.external-file=./log/dataspaceconnector.log
```

The logfile will be available by performing a pull request on `/actuator/logfile`.

For more information, see [here](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html).
