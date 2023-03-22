The communication between the Dataspace Connector and data apps can be achieved by using an integration Framework 
like Apache Camel. This also provides the possibility to use all kinds of different backends for resources registered 
in the connector, as no separate implementation has to be made for each possible protocol. To keep the Dataspace 
Connector lightweight and modular, no integration framework will be integrated directly, but rather be executed 
standalone in parallel to the connector.

This page describes how to use Apache Camel in parallel to the Dataspace Connector and gives examples for connecting 
different backend types.

## Creating Camel application

As there is no existing image for running Camel standalone, a Camel application has to be created. This example uses 
Camel with Springboot and uses the *Spring XML DSL* for making configurations and defining routes. 

At the moment, this example only shows how to define routes at start-up, but not at runtime. An addition on how to 
deploy routes dynamically will follow soon.

The application can be created by following these steps:

### 1. Create project

Create a new *Spring Initializer* project. 

### 2. Add dependencies

Add these dependencies to the project's POM:
	
	<dependency>
		<groupId>org.apache.camel.springboot</groupId>
		<artifactId>camel-spring-boot-bom</artifactId>
		<version>3.7.0</version>
		<type>pom</type>
		<scope>import</scope>
	</dependency>

	<dependency>
		<groupId>org.apache.camel.springboot</groupId>
		<artifactId>camel-spring-boot-starter</artifactId>
		<version>3.7.0</version>
	</dependency>
	
The Springboot starter contains all dependencies required to run Camel in Spring.	

### 3. Keep Camel Context running

The Camel Context is Camel's runtime system and is thus required for running any routes. Add 
```camel.springboot.main-run-controller=true``` to the project's ```application.properties``` 
(under *src/main/resources*) to keep the Camel Context running for as long as the application is running. Otherwise, it 
will shut down shortly after start-up.

### 4. Deploy routes

#### Initial/static route deployment

The easiest way to deploy routes is to statically define them in the Camel Context before the application starts.
Spring will create a default Camel Context on application start. When using that context, all beans and routes you want 
to use have to be declared separately/in different locations. By defining a Camel Context in an XML file 
(e.g. *camel-context.xml*) and importing that into the Springboot application, the whole context including routes and 
beans can be defined in one place.

The basic structure of the file looks like this:

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		   xsi:schemaLocation="
		   http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		   http://camel.apache.org/schema/spring http://camel.apache.org/schema/spring/camel-spring.xsd">

		<camelContext id="camelContext" xmlns="http://camel.apache.org/schema/spring">

		</camelContext>

	</beans>

##### Location of the Camel Context XML file

1. The XML file can be added directly to the Springboot classpath (in *src/main/resources*) and imported by adding 
```@ImportResource("classpath:camel-context.xml")``` to the application's main class. When using this approach, the 
application will have to be rebuilt every time changes to the Camel Context are made.
2. The XML file can also reside anywhere in the file system (outside the application) and can be imported by adding 
```@ImportResource("file:/some/path/camel-context.xml")``` to the application's main class. With this approach, the 
project just has to be restarted, not rebuilt, when changes to the Camel Context are made. When using this approach, 
the file has to be made available for the application under the specified path, e.g. through a mount when using Docker. 

##### Adding routes

Inside the *camelContext* tag, any desired number of routes can be added by defining them inside *route* tags:
 
	<camelContext id="camelContext" xmlns="http://camel.apache.org/schema/spring">
		<route>
			...
		</route>
	</camelContext>

Find more detailed information on how to define routes in the chapter [Defining Camel routes](#defining-camel-routes).

##### Adding beans

While some routes can run without any additional configuration, others require the declaration of beans. For example, 
a data source bean has to be defined when using a database in a route. All required beans can be defined in the 
*camel-context.xml* file as well and are thus automatically available in the Camel Context. 

**Important:** the *bean* tags have to be declared **outside** the *camelContext* tag:

	<bean id="..." class="..."></bean>

	<camelContext id="camelContext" xmlns="http://camel.apache.org/schema/spring">
		<route>
			...
		</route>
	</camelContext>

#### Dynamic route deployment

While the static deployment of routes is a simple and quick solution and will probably suffice for testing purposes, in
most cases there will be a need to deploy and/or remove routes dynamically at runtime. A possible solution is to send 
XML files containing routes and, if necessary, required beans to the Camel application via HTTP. In the 
following, an example is given on how to define the endpoints required for this approach.

In order to be able to define any HTTP endpoints in the application, add this dependency to the POM, if it isn't 
already present:

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

##### Endpoints for routes

Create a Spring *RestController* class with a field of type ```org.apache.camel.impl.DefaultCamelContext```. An 
```org.apache.camel.CamelContext``` instance can be obtained through autowiring and cast to a *DefaultCamelContext* 
instance, which will be used to add and remove routes at runtime. Then add two methods to this class, one for adding
routes read from an XML file and one for removing routes using their ID.
    
    import org.apache.camel.CamelContext;
    import org.apache.camel.impl.DefaultCamelContext;
    import org.apache.camel.model.Constants;
    import org.apache.camel.model.RoutesDefinition;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.http.HttpStatus;
    import org.springframework.http.ResponseEntity;
    import org.springframework.web.bind.annotation.DeleteMapping;
    import org.springframework.web.bind.annotation.PathVariable;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;
    import org.springframework.web.multipart.MultipartFile;
    
    import javax.xml.bind.JAXBContext;
    import javax.xml.bind.Unmarshaller;
    import java.io.InputStream;
    
    @RestController
    @RequestMapping("/routes")
    public class RoutesController {
    
        private DefaultCamelContext camelContext;
    
        @Autowired
        public RoutesController(CamelContext camelContext) {
            this.camelContext = (DefaultCamelContext) camelContext;
        }
    
        @PostMapping
        public ResponseEntity<String> addRoute(@RequestParam("file") MultipartFile file) {
            JAXBContext jaxb = JAXBContext.newInstance(Constants.JAXB_CONTEXT_PACKAGES);
            Unmarshaller unmarshaller = jaxb.createUnmarshaller();

            InputStream inputStream = file.getInputStream();
            RoutesDefinition routes = (RoutesDefinition) unmarshaller.unmarshal(inputStream);
            camelContext.addRouteDefinitions(routes.getRoutes());

            return new ResponseEntity<>("Successfully added routes to Camel Context.", HttpStatus.OK);
        }
    
        @DeleteMapping("/{routeId}")
        public ResponseEntity<String> removeRoute(@PathVariable("routeId") String routeId) {
            camelContext.stopRoute(routeId);
            camelContext.removeRoute(routeId);

            return new ResponseEntity<>("Successfully stopped and removed route with ID " + routeId, HttpStatus.OK);
        }
    }
        
*Exception handling has been omitted for brevity.*               

##### Endpoints for beans

Create another Spring *RestController* with a field of type 
```org.springframework.context.support.GenericApplicationContext```. The *GenericApplicationContext* instance can be 
obtained through autowiring and will be used to add and remove beans at runtime. Again, add two methods to this class, 
one for adding beans read from an XML file and one for removing beans using their ID. 
    
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.beans.factory.support.BeanDefinitionRegistry;
    import org.springframework.beans.factory.xml.XmlBeanDefinitionReader;
    import org.springframework.context.support.GenericApplicationContext;
    import org.springframework.http.HttpStatus;
    import org.springframework.http.ResponseEntity;
    import org.springframework.web.bind.annotation.DeleteMapping;
    import org.springframework.web.bind.annotation.PathVariable;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;
    import org.springframework.web.multipart.MultipartFile;
    import org.xml.sax.InputSource;
    
    import java.io.StringReader;
    
    @RestController
    @RequestMapping("/beans")
    public class BeansController {
    
        private GenericApplicationContext applicationContext;
    
        @Autowired
        public BeansController(GenericApplicationContext applicationContext) {
            this.applicationContext = applicationContext;
        }
    
        @PostMapping
        public ResponseEntity<String> addBeans(@RequestParam("file") MultipartFile file) {
            String xml = new String(file.getBytes());
            XmlBeanDefinitionReader xmlBeanDefinitionReader = new XmlBeanDefinitionReader(applicationContext);
            xmlBeanDefinitionReader.setValidationMode(XmlBeanDefinitionReader.VALIDATION_XSD);
            xmlBeanDefinitionReader.loadBeanDefinitions(new InputSource(new StringReader(xml)));

            return new ResponseEntity<>("Successfully added beans to Application Context.", HttpStatus.OK);
        }
    
        @DeleteMapping("/{beanId}")
        public ResponseEntity<String> removeBean(@PathVariable("beanId") String beanId) {
            BeanDefinitionRegistry registry = (BeanDefinitionRegistry) applicationContext.getAutowireCapableBeanFactory();
            registry.removeBeanDefinition(beanId);

            return new ResponseEntity<>("Successfully removed bean with ID " + beanId, HttpStatus.OK);
        }
    } 
        
*Exception handling has been omitted for brevity.* 

##### Adding routes and beans at runtime

When the two *RestController* classes are implemented, you can now send XML files containing routes or beans to the 
Camel application via HTTP using the paths defined in the *RestController* classes. They will be automatically added 
to the context and, in case of routes, also started.

The top-level tag for files containing routes must be the *routes* tag:

    <routes xmlns="http://camel.apache.org/schema/spring">
    
        <route id="...">
            ...
        </route>
        
    </routes>
    
The top-level tag for files containing beans must be the *beans* tag:    

    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="
           http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
           http://camel.apache.org/schema/spring http://camel.apache.org/schema/spring/camel-spring.xsd">

        <bean id="..." class="...">
            ...
        </bean>    
         
    </beans> 
    
The files may contain more than one route or bean. In that case, all listed routes or beans will be added. If a route
requires a certain bean to be present, the bean has to be added first! Otherwise, the route deployment will fail 
with an ```org.apache.camel.FailedToCreateRouteException``` caused by an ```org.apache.camel.NoSuchBeanException```.  

## Defining Camel routes

Each Camel route has to contain one start point (*&lt;from uri="..."&gt;*) and at least one endpoint to send a message 
to (*&lt;to uri="..."&gt;*). For these, any Camel component can be used (e.g. *http*, *sql*). Each component used in 
the routes has to be added to the Camel application's POM as a dependency. For this, the component's Springboot starter 
can be used. As all of the routes should either fetch data from or send data to the Dataspace Connector using HTTP, the 
Camel HTTP component should be added in any case:

	<dependency>
		<groupId>org.apache.camel.springboot</groupId>
		<artifactId>camel-http-starter</artifactId>
		<version>3.7.0</version>
	</dependency>

Hereinafter, example routes are given for using different Camel components as data sources/sinks. Depending on whether
you're using [static](#initialstatic-route-deployment) or [dynamic](#dynamic-route-deployment) route deployment, the 
routes (and beans) have to either be added to *camel-context.xml* or sent to the Camel application at runtime. 

### Provider

The following examples show how to fetch data from different backend systems and publish it to the Dataspace Connector 
as a resource's data. In all cases, a resource with the specified ID has to be created in the connector first.

#### HTTP

As the Camel HTTP component should already have been added to the POM, no further dependencies are required. Using an 
HTTP backend also does not require any additional beans to be defined. Therefore, only the route definition has to 
added.

The Camel HTTP component cannot be used as a consumer, i.e. that it cannot be used in a *from* tag and thus cannot be 
used as the start of a route. A simple workaround is to use a timer as the route start and then call the HTTP backend.

    <route id="http-to-dsc-example">

        <from uri="timer://foo?delay=10000&amp;period=15000"/>

        <setHeader name="CamelHttpMethod"><constant>GET</constant></setHeader>
        <to uri="http://http-demo-backend:8090/demo"/>
        
        <convertBodyTo type="java.lang.String"/>

        <setHeader name="CamelHttpMethod"><constant>PUT</constant></setHeader>
        <setHeader name="Authorization"><constant>Basic YWRtaW46cGFzc3dvcmQ=</constant></setHeader>
        <to uri="http://dataspace-connector:8080/admin/api/resources/3bc8731a-0d82-4899-a3a6-88ab10f31223/data"/>

    </route>
	
This route will start every 15 seconds (with an initial delay of 10 seconds), make an HTTP GET call to the backend and 
send that to the connector as the data of the resource with ID *3bc8731a-0d82-4899-a3a6-88ab10f31223*.

#### SQL 

To use a relational database as the data source, the Camel SQL component has to be added to the POM:

	<dependency>
		<groupId>org.apache.camel.springboot</groupId>
		<artifactId>camel-sql-starter</artifactId>
		<version>3.7.0</version>
	</dependency>
	
For Spring to be able to load the database driver for the database connection, the dependency of the chosen database 
management system (e.g. PostgreSQL, MySQL) has to be added to the POM as well.	
	
When using the Camel SQL component, the database to use has to be configured in a bean. Therefore, add a data source 
bean:

	<bean id="testDataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="url" value="jdbc:postgresql://postgres:5432/testdb"/>
		<property name="driverClassName" value="org.postgresql.Driver" />
		<property name="username" value="postgres-user" />
		<property name="password" value="12345"/>
	</bean>

Afterwards, the route definition can be added. The query to use when fetching data from the database has to be specified
in the URI of the *from* tag:

    <route id="sql-to-dsc-example">

        <from uri="sql:select * from test?initialDelay=10000&amp;delay=15000&amp;useIterator=false&amp;dataSource=#testDataSource"/>
        
        <convertBodyTo type="java.lang.String"/>

        <setHeader name="CamelHttpMethod"><constant>PUT</constant></setHeader>
        <setHeader name="Authorization"><constant>Basic YWRtaW46cGFzc3dvcmQ=</constant></setHeader>
        <to uri="http://dataspace-connector:8080/admin/api/resources/3bc8731a-0d82-4899-a3a6-88ab10f31223/data"/>

    </route>

This route will start every 15 seconds (with an initial delay of 10 seconds), fetch data from the database using the 
specified query and send that to the connector as the data of the resource with ID 
*3bc8731a-0d82-4899-a3a6-88ab10f31223*.
	
Note that the data source bean's ID is referenced as the value for the query parameter *dataSource* in the *from* tag. 
This allows you to specify the correct data source if multiple data source beans are defined. There is also a query 
parameter *useIterator* which is set to *false*. This defines that all records returned by the query will be fetched and 
sent in **one** message. If the value is set to *true*, **each** record returned by the query will be sent in a 
**separate** message.

#### MQTT

To use an MQTT broker as the data source, the Camel MQTT component has to be added to the POM. As no Springboot starter 
is available for that component, the Paho component can be used instead. It also offers support for MQTT, but is, as 
opposed to the MQTT component, implemented using the *Eclipse Paho* library. Add the following dependency to the POM:

	<dependency>
		<groupId>org.apache.camel.springboot</groupId>
		<artifactId>camel-paho-starter</artifactId>
		<version>3.7.0</version>
	</dependency> 
	
Using the Paho component does not require additional beans to be defined, as all information required for the 
connection can be given in the URI of the *from* tag. Therefore, only the route has to added:

    <route id="mqtt-to-dsc-example">

        <from uri="paho:test-topic?brokerUrl=tcp://mosquitto:1883"/>
        
        <convertBodyTo type="java.lang.String"/>

        <setHeader name="CamelHttpMethod"><constant>PUT</constant></setHeader>
        <setHeader name="Authorization"><constant>Basic YWRtaW46cGFzc3dvcmQ=</constant></setHeader>
        <to uri="http://dataspace-connector:8080/admin/api/resources/3bc8731a-0d82-4899-a3a6-88ab10f31223/data"/>

    </route>

This route will be triggered whenever a new message is published to the topic with name *test-topic* at the MQTT broker 
located at *tcp://mosquitto:1883* and send that message's payload to the connector as the data of the resource with ID
*3bc8731a-0d82-4899-a3a6-88ab10f31223*.		

### Consumer

The following example shows how to fetch a resource's data from the connector and send it to a backend system.

#### File

For Camel to be able to write to a file, the Camel file component has to be added to the POM:

	<dependency>
		<groupId>org.apache.camel.springboot</groupId>
		<artifactId>camel-file-starter</artifactId>
		<version>3.7.0</version>
	</dependency>

No additional beans have to be defined to use the file component, so only the route has to be added. As the route starts
with fetching data from the connector using HTTP and the Camel HTTP component cannot be used as a consumer, a timer 
is used as the route start.

    <route id="dsc-to-file-example">

        <from uri="timer://foo?delay=10000&amp;period=15000"/>

        <setHeader name="CamelHttpMethod"><constant>POST</constant></setHeader>
        <setHeader name="Authorization"><constant>Basic YWRtaW46cGFzc3dvcmQ=</constant></setHeader>
        <to uri="http://dataspace-connector:8080/admin/api/resources/3bc8731a-0d82-4899-a3a6-88ab10f31223/data"/>
        
        <convertBodyTo type="java.lang.String"/>

        <to uri="file:/output?fileName=resourcedata.txt"/>

    </route>
	
This route will start every 15 seconds (with an initial delay of 10 seconds), fetch the data of the connector's 
resource with ID *3bc8731a-0d82-4899-a3a6-88ab10f31223* and write it to a file located at */output/resourcedata.txt*.	
	
## Using the Dataspace Connector with SSL enabled

In all given example routes the Dataspace Connector is addressed using HTTP, not HTTPS. This is due to the fact that 
in test environments self signed certificates are often used. These lead to an error when Camel tries to call the 
connector. A workaround is either disabling SSL or implementing a custom 
*org.apache.camel.component.http.HttpClientConfigurer* that does not fail with self signed certificates, declare it as 
a [bean](#adding-beans) and add it as a query parameter at the end of the connector URI:

	uri="https://dataspace-connector:8080/...?httpClientConfigurer=#idOfHttpClientConfigurerBean"/>
	
**These workarounds should only be used in test environments. In productive environments SSL should always be enabled 
and real certificates should be used!**	

## Example setup 

To run the newly created Camel application with the Dataspace Connector, the following *docker-compose.yaml* can be 
used (note that this requires a Dockerfile to be added to the Camel application first):

	version: '3'
	services:
	
	  dataspace-connector:
		container_name: dataspace-connector
		build:
		  path/to/DataspaceConnector
		ports:
		  - "8080:8080"
		environment:
		  - SERVER_SSL_ENABLED=false
		  
	  camel:
		container_name: camel
		build:
		  path/to/CamelApplication
		ports:
		  - "9090:9090"
		volumes:  
		  - ./path/to/camel-context.xml:/some/path/camel-context.xml

The *camel-context.xml* has to be mounted to the Camel application container under the same path specified in the Camel 
application's main class (*@ImportResource("file:/some/path/camel-context.xml")*). In the *docker-compose.yaml* you can 
also add all services required for the routes, like a database or an MQTT broker.

## Using apps

When using Camel, data apps can be integrated into the routes. In the following, it will be described how to achieve 
this. If possible, a data app should provide one HTTP endpoint for both input and output. The data transferred in a 
route can therefore be sent to this endpoint using the Camel HTTP component. The app's result will then be synchronously 
returned as the response. Besides the actual data, an app may also require information about the usage policy associated 
with this data to be able to enforce usage control. To not be bound to a specific payload format, this additional 
information is not sent as part of the payload, but rather added in HTTP headers. Thus, it is still sent in the same 
request as the corresponding data, but is clearly separated from it. Camel offers the possibility to set Camel headers 
for a message, that in case of communicating via HTTP will be mapped to HTTP headers once an endpoint is called. The 
Camel headers can therefore be used for setting the additional information.

The additional headers for calling a data app are:
- **TargetDataUri**: URI of the artifact that is being transferred
- **ContractId**: ID of the contract associated with the transferred artifact
- **AppName**: name of the app the message is sent to
- **AppUri**: URI of the app the message is sent to

Based on this, a route that includes a data app looks as follows:

    <route id="backend-to-app-to-dsc-example">
    
        <from uri="timer://foo?delay=10000&amp;period=20000"/>
        <setHeader name="CamelHttpMethod"><constant>GET</constant></setHeader>
        <to uri="http://http-demo-backend:8090/demo"/>
        <convertBodyTo type="java.lang.String"/>

        <setHeader name="TargetDataUri">
            <constant>
                http://dataspace-connector:8080/admin/api/resources/3bc8731a-0d82-4899-a3a6-88ab10f31223
            </constant>
        </setHeader>
        <setHeader name="ContractId">
            <constant>
                https://w3id.org/idsa/autogen/contractOffer/591467af-9633-4a4e-8bcf-47ba4e6679ea
            </constant>
        </setHeader>
        <setHeader name="AppName"><constant>Demo App</constant></setHeader>
        <setHeader name="AppUri"><constant>http://demo-app:5000</constant></setHeader>
        
        <setHeader name="CamelHttpMethod"><constant>POST</constant></setHeader>
        <to uri="http://demo-app:5000?socketTimeout=10000"/>

        <setHeader name="CamelHttpMethod"><constant>PUT</constant></setHeader>
        <setHeader name="Authorization"><constant>Basic YWRtaW46cGFzc3dvcmQ=</constant></setHeader>
        <to uri="http://dataspace-connector:8080/admin/api/resources/3bc8731a-0d82-4899-a3a6-88ab10f31223/data"/>

    </route>
    
This example route fetches data from an HTTP backend, sets the usage control relevant headers using the *setHeader* tag
and then sends the data and the headers to a data app. The response returned by the app is sent to the Dataspace 
Connector as the data of an existing resource.    