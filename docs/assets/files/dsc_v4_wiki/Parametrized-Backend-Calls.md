The Dataspace Connector supports the usage of custom headers, query parameters and path variables when fetching data 
from backend systems. Therefore, it is possible for a data consumer to specify them when sending an 
ArtifactRequestMessage. 

## Provider

An additional field *endpointDocumentation* of type URI has been added to the class *ResourceMetadata*. Thus, when 
registering a resource, a data provider can now add a link pointing to the API documentation for the referenced backend. 
In that documentation the path variables, query parameters and headers expected or supported by the backend API should 
be described, if any.

A few things have to be considered when defining the URL of the backend:

* If the data consumer should be able to choose query parameters (i.e. if the API documentation lists any), 
no query parameters must be present in the initial URL. All query parameters must then be chosen solely by the data 
consumer.
    - Allowed: `http://example-backend.com`
    - Not allowed: `http://example-backend.com?param=value`  
* If the backend URL contains path variables, they have to be enclosed by curly brackets. The name between the curly 
brackets must match the variable name from the API documentation, as the data consumer will specify the value using 
that name. 
    - Example: `http://example-backend.com/{pathVariable1}/{pathVariable2}`

To test the backend call with headers, query parameters and/or path variables, the resource data can now be queried 
using an optional request body under ```POST /admin/api/resources/{resourceId}/data```. Find more detailed information 
on the request body in the chapter below.

## Consumer

After requesting a resource's metadata, a data consumer can now follow the link to the documentation, decide which 
headers, query parameters and path variables they have to and want to use and then specify them in the request body when 
requesting the artifact. The request body should look as follows:

	{
	  "headers": {
	    "key1": "value1",
	    "key2": "value2"
	  },
	  "params": {
	    "key1": "value1",
	    "key2": "value2"
	  },
	  "pathVariables": {
	    "key1": "value1",
	    "key2": "value2"
	  }
	}

Note that the request body is optional and can be omitted all together if no headers, query parameters and path 
variables should be supplied. It is also possible to partly omit the request body, i.e. the following will also work:

	{
	  "params": {
	    "key": "value"
	  }
	}

## Examples

### Query parameters

#### Scenario 

A data provider wants to share data from a REST API that uses query parameters. They don't want to create an additional
representation for each possible combination of query parameters, but instead offer the API's data using one single
representation and letting the data consumer decide on the query parameters they want to use.

#### Steps

1. Provider defines the base URL of the REST API as the URL in the resource representation:
    `http://example-backend.com` 
2. Provider provides a link to the REST API documentation in the field *endpointDocumentation* of the resource's 
metadata.
    ```
        Query parameters according to API documentation: 
            required: name (any string)
            optional: number (any positive integer)
    ```
3. Consumer follows the link to the REST API documentation after requesting the resource's metadata.
4. Consumer specifies the query parameters in the request body when sending an artifact request. Here, all values have
to be declared as strings, even if the query parameter is e.g. of type integer.
    1) only the required parameter
    ```
        {
          "params": {
            "name": "John"
          }
        }
    ```
    2) the required and the optional parameter
    ```
        {
          "params": {
            "name": "John",
            "number": "3"
          }
        }
    ```
5. After receiving the ArtifactRequestMessage, the provider connector fetches the data from the backend system after 
appending the specified query parameters to the URL. 
    1) `http://backend.com?name=John`
    2) `http://backend.com?name=John&number=3`
     	
### Path variables	

#### Scenario
 
A data provider wants to share data from a REST API that uses path variables. They don't want to create an additional
representation for each possible path variable value or each possible combination of values, but instead offer the 
API's data using one single representation and letting the data consumer decide which path variable value(s) they want 
use.

#### Steps

1. Provider defines the URL of the REST API as the URL in the resource representation, where the path variables
are enclosed by curly brackets:
    `http://backend.com/{name}/{number}` 
2. Provider provides a link to the REST API documentation in the field *endpointDocumentation* of the resource's 
metadata.  
    ```
       Path variables according to API documentation:
            required: name (any string), number (any positive integer)
    ```
3. Consumer follows the link to the REST API documentation after requesting the resource's metadata.
4. Consumer specifies the path variables in the request body when sending an artifact request. Here, all values have 
to be declared as strings, even if the path variable is e.g. of type integer. 
    ```
        {
          "pathVariables": {
            "name": "John",
            "number": "3"
          }
        }
   ```
5. After receiving the ArtifactRequestMessage, the provider connector fetches the data from the backend system after 
replacing the path variables in the URL.
    `http://backend.com/John/3`