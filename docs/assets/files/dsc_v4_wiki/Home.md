**Welcome to the Dataspace Connector Wiki!** 

* [Database Configuration](https://github.com/FraunhoferISST/DataspaceConnector/wiki/database-configuration): On this page, you can find some more details on how to replace the built-in database.
* [Deployment](https://github.com/FraunhoferISST/DataspaceConnector/wiki/deployment): Here, you can find a detailed guide for developing and deploying the software.
* [Example JSON Strings](https://github.com/FraunhoferISST/DataspaceConnector/wiki/examples): Whether policies or IDS messages, this page shows examples of IDS metadata. 
* [FAQ](https://github.com/FraunhoferISST/DataspaceConnector/wiki/faq): You have encountered an apparently unsolvable problem? Maybe someone else has already found a solution.
* [Getting Started](https://github.com/FraunhoferISST/DataspaceConnector/wiki/getting-started): Get an example setup running without diving into the code. 
* [Hands on IDS Communication](https://github.com/FraunhoferISST/DataspaceConnector/wiki/ids-communication-guide): This page explains how to use the API of the connector.
* [IDS Connector Architecture](https://github.com/FraunhoferISST/DataspaceConnector/wiki/ids-connector-architecture): Connector information from the reference architecture model at a glance.
* [Kubernetes](https://github.com/International-Data-Spaces-Association/DataspaceConnector/wiki/Kubernetes): Example and description for deploying the Dataspace Connector in Kubernetes
* [Logging](https://github.com/FraunhoferISST/DataspaceConnector/wiki/logging): Here, you can find a detailed description on how to use built-in logging functionality.
* [Parametrized Backend Calls](https://github.com/International-Data-Spaces-Association/DataspaceConnector/wiki/Parametrized-backend-calls): Here, you can find details on how to use dynamic URLs for backends.
* [Roadmap](https://github.com/FraunhoferISST/DataspaceConnector/wiki/roadmap): Ensuring developments following the same goal, the project's roadmap and each aspect are described in detail.
* [Software Documentation](https://github.com/FraunhoferISST/DataspaceConnector/wiki/software-documentation): For the developers, some software documentation is provided.
* [Software Tests](https://github.com/FraunhoferISST/DataspaceConnector/wiki/software-tests): Here, you can find more details about tests.
* [Usage Control](https://github.com/FraunhoferISST/DataspaceConnector/wiki/usage-control): Usage policies are an important aspect of IDS, further details are explained on this page.
* [Using Camel](https://github.com/International-Data-Spaces-Association/DataspaceConnector/wiki/Using-Camel): Here, you can find instructions for using Camel with the Dataspace Connector.

## IDS-ready

<img width="240" height="271" align="right" src="https://www.isst.fraunhofer.de/de/news/pressemitteilungen/2020/Dataspace-Connector/jcr:content/contentPar/pressarticle/pressArticleParsys/textwithasset/imageComponent/image.img.4col.png/1608540266652/ids-ready.png">

"The aim of the Dataspace Connector is to provide companies with an easy and trustworthy entry into 
the International Data Spaces. There are three levels of certification for the International Data 
Spaces, an initiative for cross-industry data exchange with over 100 European companies: Base, 
Trusted and Trusted+. The DSC was deliberately tested for the Base certification level, as this does 
not require specific hardware such as Trusted Platform Module chips in order to use the connector. 
This makes it easier to use the DSC on different hardware and in cloud environments with a reasonable 
sacrifice of hardware security features. 

In addition, the Dataspace Connector is the only IDS connector that already supports the enforcement 
of eight usage condition classes of the International Data Spaces Association and thus exceeds the 
Base certification level."

— [News Release](https://www.isst.fraunhofer.de/de/news/pressemitteilungen/2020/Dataspace-Connector.html)

## Supported Features

This is a list of currently implemented features, which is continuously updated.

*  Settings for TLS, proxy and Spring Boot basic authentication for backend endpoints
*  Use valid IDS certificate and request DAT from DAPS
*  Data resource registration (CRUD metadata) with internal H2 database or an external database
*  Possibility to add multiple representations (different backend connections) to a resource
*  Backend data handling internal (CRUD data) with internal H2 database or an external database
*  Backend data handling external via `http GET` or `https GET` (with basic auth)
*  IDS message handling with other IDS Connectors (as data provider and data consumer): 
`DescriptionRequestMessage`, `DescriptionResponseMessage`, `ArtifactRequestMessage`, `ArtifactResponseMessage`, 
`ContractRequestMessage`, `ContractAgreementMessage`, `ContractRejectionMessage`, `RejectionMessage`
*  Read IDS response messages: deserialize and save requested data & metadata in internal database
*  IDS message handling with the IDS Broker (IDS Lab): `ConnectorUpdateMessage`, `ConnectorUnavailableMessage`, 
`ResourceUpdateMessage`, `ResourceUnavailableMessage`, `QueryMessage`
* Further supported message types: sending `NotificationMessage` and `LogMessage`, receiving `NotificationMessage` 
* Receiving `ResourceUpdateMessage` and updating the data automatically.
*  Usage control with 9 ODRL policy patterns following the IDS policy language specifications: 
`provide access`, `prohibit access`, `usage during interval`, `n times usage`, `duration usage`, 
`usage until deletion`, `usage logging`, `usage notification`, `connector-restricted usage`

### Core Functionality

For roadmap details, see [here](https://github.com/FraunhoferISST/DataspaceConnector/wiki/roadmap#core-functionality).

| Task                       | Responsible | Status  | Description                                  | Note |
|:---------------------------|:------------|:--------|:---------------------------------------------|:-----|
| Exception Handling         | ISST        | done    | [here](https://github.com/FraunhoferISST/DataspaceConnector/wiki/roadmap#exception-handling)        |      |
| Logging                    | ISST        | done    | [here](https://github.com/FraunhoferISST/DataspaceConnector/wiki/roadmap#logging)                   |      |
| Additional Configurations | ISST        | done    | |

### IDS Functionality

For roadmap details, see [here](https://github.com/FraunhoferISST/DataspaceConnector/wiki/roadmap#ids-functionality).

| Task                        | Responsible | Status | Description                                   | Note |
|:----------------------------|:------------|:-------|:----------------------------------------------|:-----|
| Ids-ready Test              | ISST        | done   | [here](https://github.com/FraunhoferISST/DataspaceConnector/wiki/roadmap#ids-ready-test)             |      |
| Config Manager Integration  | ISST        | done   | [here](https://github.com/FraunhoferISST/DataspaceConnector/wiki/roadmap#config-manager-integration) |      |
| Basic Policy Negotiation    | ISST        | done   | [here](https://github.com/FraunhoferISST/DataspaceConnector/wiki/roadmap#basic-policy-negotiation)   |      |
| Support Resource Update Messages    | ISST        | done   | |
| Backend Parametrization    | ISST        | done   | [here](https://github.com/International-Data-Spaces-Association/DataspaceConnector/wiki/Parametrized-Backend-Calls) |