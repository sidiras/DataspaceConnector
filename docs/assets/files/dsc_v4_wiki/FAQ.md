Please take a look at the [issues](https://github.com/FraunhoferISST/DataspaceConnector/issues) and 
[discussions](https://github.com/FraunhoferISST/DataspaceConnector/discussions) as well.

## Connector Deployment

**Is it necessary to use the IDS Framework for deploying the Dataspace Connector?**

If the application is to be started as it is in the repository, then yes. All IDS communication is 
implemented by the [IDS Framework](https://github.com/FraunhoferISST/IDS-Connector-Framework). 
In general, however, the corresponding functions can also be implemented in the connector, so that 
using the IDS Framework is no longer required.

**Why do I get a `{"error":"invalid_client","error_description":""}` error when I start the application?**

This occurs when the connector could not get a valid access token by the DAPS. A possible reason 
could be a deviation in your system time. If this time is ahead of the tolerated time of the DAPS, 
the JSON Web Token, that is sent to the DAPS when the application is started, is not accepted and an 
error is received as a result. Please check your system time at https://time.is/. If the deviation 
exceeds 10 seconds, the time in the class `TokenProvider` has to be changed (increased).

```
.setIssuedAt(Date.from(Instant.now().minusSeconds(10L)))
.setAudience("ids_connector")
.setNotBefore(Date.from(Instant.now().minusSeconds(10L)))
```

## Resource Handling

**How to connect the Dataspace Connector to an external database or system?**

The data retrieval in the background, i.e. the communication between Dataspace Connector and backend 
system or database, is performed without IDS. All information about where to find the data is 
included in the data resource's metadata. The connector uses this information to establish a 
connection to the data query. If no simple GET endpoint (as implemented) is available in the backend, 
an interface for another source typ can be implemented and the method 
`OfferedResourceService.getDataString()` and linked methods changed accordingly.


## IDS Communication

**Why do I have to provide a validation key when submitting a data request?**

As the metadata provide the contract settings, including usage policies, specifying the data usage, 
it is important that the data consumer can actively request and process it. 

**Why do I get a PKIX exception?**
 
The certificate of the requested URL might not be trustworthy. External URLs are checked against the 
truststore placed at `resources/conf` by the IDS Framework. To avoid problems with an externally 
running instance of the Dataspace Connector, the SSL settings of the external application should 
either be disabled in the `application.properties`, or its keystore should be replaced by the IDS 
keystore with the IDS Connector certificate. For any other URL, you need to extend your truststore. 
Have a look at [configuration settings](https://github.com/FraunhoferISST/DataspaceConnector/wiki/development#configurations) for more details.

**Why do I get a Rejection Message `Rejection Message: Token could not be parsed!`?**

The Dataspace Connector returns this error when the `ids:connectorDeployMode` was set to 
`idsc:PRODUCTIVE_DEPLOYMENT` without using a valid IDS keystore. Have a look at 
[configuration settings](https://github.com/FraunhoferISST/DataspaceConnector/wiki/development#configurations) for more details.

**There is a error (`ERROR TokenManagerService:166 - Something else went wrong:`) thrown in my log 
every time I am sending an IDS message, although I am getting correct response messages.**

This error is thrown by the IDS Framework and will be printed as long as the `ids:connectorDeployMode` 
is set to `idsc:TEST_DEPLOYMENT`. However, the message handling will keep working for testing purpose.

**Why do I get a `RejectionMessage` `NOT_AUTHENTICATED` when requesting the metadata or data from 
an external connector?**

Check if the DAT token of the data consumer is still valid. If an IDS connector receives an 
IDS message, the attached token is checked. If it is not valid, the connector responds with a `RejectionMessage`. 

**Why do I get a `RejectionMessage` `NOT_AUTHORIZED` when requesting the data from an external connector?**

Check the defined policy. If the internal policy check detects that the consumer, for whatever reason, 
is not allowed to access the data, a `RejectionMessage` is returned.


**Why do I get a `RejectionMessage` `NOT_FOUND` when requesting the metadata or data from an external connector?**

This occurs when there is no data resource with the requested URI saved in the internal database. 
Please check the requested URI. Note that a data resource that contains requested data from another 
connector is stored as `internal`. If the data consumer connector itself becomes the data provider, 
the same resource cannot be retrieved by other connectors. If the error still occurs, contact the 
developer mentioned above.


## Other

**How can the IDS specific strings be deserialized as they are not plain JSON? Does an appropriate 
parser has to be implemented for this?**

The extracted string in the connector's response body is JSON-LD not plain JSON. To avoid having to 
write your own parser, the IDS Infomodel already offers one. The extracted JSON-LD string, be it the 
resource or the connector's self-description, can be automatically parsed to Java objects with the 
help of the provided [IDS Infomodel Serializer](https://maven.iais.fraunhofer.de/artifactory/eis-ids-public/de/fraunhofer/iais/eis/ids/infomodel-serializer/). 
Simply take the complete JSON(-LD) String and, e.g., use the following lines:

```
Serializer serializer = new Serializer(); 
BaseConnector connector = serializer.deserialize(string, BaseConnector.class);
```

**Does the Dataspace Connector act event-driven?**

No, it is based on a request-response concept.


**Does the Dataspace Connector work exactly like the Trusted Connector?**

The Dataspace Connector is **not** a Trusted Connector. Instead, it is of the `BaseConnector` type. 
A detailed explanation can be found [here](https://github.com/FraunhoferISST/DataspaceConnector/discussions/74).
