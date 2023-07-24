# General CAP Collection

Also refer to : https://github.com/atanumallik1/CAP-Learning
- Cap Recording
  - https://recap-conf.dev/   

- Application Lifecycle Service
  - https://cap.cloud.sap/docs/java/consumption-api#application-lifecycle-service
  - Handling error messages using application lifecycle handler https://cap.cloud.sap/docs/java/indicating-errors#errorhandler
- Persistance Service APIs
  - https://cap.cloud.sap/docs/java/persistence-services#default-persistence-service  
## Working with CDS mdoel 
- https://cap.cloud.sap/docs/java/reflection-api#get-all-elements-with-given-annotation
## API 
- Stop a Service from exposure 
   -  `@cds.serve.ignore`
- dont create DB
   - `	@cds.persistence.skip`
   - 
## REst Protocol
- https://cap.cloud.sap/docs/node.js/cds-serve#protocol
- https://cap.cloud.sap/docs/advanced/troubleshooting#how-can-i-expose-custom-rest-apis-with-cap
- Protocol specific endpoint : https://cap.cloud.sap/docs/releases/jun23#new-protocol-specific-service-endpoints
## Open Types 
- https://cap.cloud.sap/docs/releases/apr23#open-types-in-odata-v4

## DB Interaction 
- Bulk API
  - https://cap.cloud.sap/docs/java/query-api#bulk-insert
  - https://cap.cloud.sap/docs/java/query-api?q=bulk+update#bulk-update 
- Row Level locking 
  - Pessimistic locking : https://cap.cloud.sap/docs/guides/providing-services/#pessimistic-locking
  - ````cds
        Select<Cart_> select = Select.from(Cart_.class)
        .columns(r -> r._all(), r -> r.LineItems()
            .expand(i -> i._all(), i -> i.AccountAssignments()
                    .expand(),
                i -> i.Comments()
                    .expand()
                    .orderBy(c -> c.modifiedAt()
                        .asc())))
        .where(row -> row.get(Cart.CREATED_BY)
            .eq(eventContext.getUserInfo()
                .getName()));
      select = select.lock();
      CqnSelect selectQuery = select.asSelect();
    ````  
## Connecting to Secondary Data Source using CDSDataSourceConnector 
- https://cap.cloud.sap/docs/java/advanced
## Transactional Outbox 
- https://cap.cloud.sap/docs/java/outbox
## Audit Logging 
- https://cap.cloud.sap/docs/java/auditlog

## Development 
### Application Configuration
- https://cap.cloud.sap/docs/java/development/#application-configuration
- This section describes how to configure applications. CAP Java applications can fully leverage Spring Boot's capabilities for Externalized Configuration. This enables you to define multiple configuration profiles for different scenarios, like local development and cloud deployment.
- For a first introduction, have a look at our sample application and the configuration profiles we added there.
- Now, that you’re familiar with how to configure your application, start to create your own application configuration. See the full list of CDS properties as a reference.
### Kyma based consumption 
- https://cap.cloud.sap/docs/java/development/#prepare-your-cap-application
- Explains how to bind to Kubernetes volumes

## Spring boot integration
- https://cap.cloud.sap/docs/java/development/#spring-boot-integration
### Spring Dependencies
- spring-boot-starter-web
- spring-boot-starter-jdbc
- spring-boot-starter-security (optional)
  or
- cds-starter-spring-boot-odata ( comes with protocol adapter )

### Consuming /Spring features
- https://cap.cloud.sap/docs/java/development/#spring-features

### Spring boot exposed beans which can be used with CDS 
- https://cap.cloud.sap/docs/java/development/#exposed-beans
- 

## Building CAP Java Applications

### Maven build options 
- https://cap.cloud.sap/docs/java/development/#maven-build-options

### Increased developer efficiency with Spring Boot Devtools
- https://cap.cloud.sap/docs/java/development/#increased-developer-efficiency-with-spring-boot-devtools
- explaisn how to use  Spring Boot Devtools
### CDS Maven plugin
- https://cap.cloud.sap/docs/java/development/#cds-maven-plugin

### Local Development Support
- https://cap.cloud.sap/docs/java/development/#local-development-support : It describes teh following 
- CDS Watch for java
- CDS auto Build

## Testing Cap Java application 
### Event Handler Layer Testing
- https://cap.cloud.sap/docs/java/development/#event-handler-layer-testing

### Service layer Testing 
- https://cap.cloud.sap/docs/java/development/#service-layer-testing
- Using CQN CRUD API
- Esing emit API
- Exception testing
- 
### API Integration Testing
- https://cap.cloud.sap/docs/java/development/#api-integration-testing
- We can uset using MockMVC

## Messaging 
- https://cap.cloud.sap/docs/java/messaging-foundation
- 
## Remote Service consumption 
- https://cap.cloud.sap/docs/java/remote-services
- Destination configuration
  - https://cap.cloud.sap/docs/java/remote-services#register-destinations ( Oauth 2, Token forwarding..)
 
## Security 
- https://cap.cloud.sap/docs/java/security
  - Authentication
  - Configure XSUAA Authentication
  - Configure IAS Authentication
  - Configure IAS and XSUAA Authentication (Hybrid)
  - Automatic Spring Boot Security Configuration
  - Setting the Authentication Mode
    - never
    - model-relaxed
    - model-strict
    - always 
  - Customizing Spring Boot Security Configuration
  - Custom Authentication
  - Mock User Authentication with Spring Boot
    - Preconfigured Mock Users
    - Explicitly Defined Mock Users
  - Mock Tenants
  - Authorization
    - Role-Based Authorization
    - Instance-Based Authorization
    - Enforcement API & Custom Handlers

## ChangeSet Contexts : Working with DB transactions 
ChangeSet Contexts are an abstraction around transactions. This chapter describes how ChangeSets are related to transactions and how to manage them with the CAP Java SDK.

- https://cap.cloud.sap/docs/java/changeset-contexts


ChangeSet Contexts are used in the CAP Java SDK as a light-weight abstraction around transactions. They are represented by the ChangeSetContext interface. ChangeSet Contexts only define transactional boundaries, but do not define themselves how a transaction is started, committed or rolled back. They are therefore well suited to plug in different kinds of transaction managers to integrate with different kinds of transactional resources.

The currently active ChangeSet Context can be accessed from the Event Context: `context.getChangeSetContext();`

- Defining ChangeSet Contexts
- Reacting on ChangeSets
- Cancelling ChangeSets
- Database Transactions in Spring Boot
  - Ref: https://cap.cloud.sap/docs/java/changeset-contexts#database-transactions-in-spring-boot
  - Database transactions in CAP are always started and initialized lazily during the first interaction with the Persistence Service. When running in Spring Boot, CAP Java completely integrates with Spring's transaction management. As a result you can use Spring's @Transactional annotations or the TransactionTemplate to control transactional boundaries as an alternative to using the ChangeSet Context.

  - This integration with Spring's transaction management also comes in handy, in case you need to perform plain JDBC connections in your event handlers. This might be necessary, when calling SAP HANA procedures or selecting from tables not covered by CDS and the Persistence Service. 
- Setting Session Context Variables
  -  You can leverage the simplified access to JDBC APIs in Spring Boot to set session context variables on the JDBC connection. When setting these variables this way, they will also influence statements executed by CAP itself through the Persistence Service APIs.
  -  Ref: https://cap.cloud.sap/docs/java/changeset-contexts#setting-session-context-variables 


## Request Contexts
Request Contexts span the execution of multiple events on (different) services. They provide a common context to these events, by providing user or tenant information or access to headers or query parameter.
- Ref: https://cap.cloud.sap/docs/java/request-contexts

### Reading Request Contexts
The Request Context provides information about the request's parameters as well as the current user:

- UserInfo: Exposes an API to fetch data of the (authenticated) user such as the logon name, id, CAP roles, and the tenant.
- ParameterInfo: Exposes an API to retrieve additional request data such as header values, query parameters, and the locale.
- AuthenticationInfo: Exposes an API to retrieve the authentication claims according to the authentication strategy. Can be used for user propagation.
- FeatureTogglesInfo: Exposes an API to retrieve activated feature toggles of the request.
- In addition, it provides access to the CDS model, which specifically can be dependent on tenant information or feature toggles.


### Defining Request Contexts
The CAP Java SDK allows you to create new Request Contexts and define their scope. This helps you to control, which set of parameters is used when events are processed by services. To manually add, modify or reset specific attributes within the scope of a new Request Context, you can use the RequestContextRunner API.

Refer to [this](https://cap.cloud.sap/docs/java/request-contexts#defining-requestcontext) for details
  
### Registering Global Providers

Refer to : https://cap.cloud.sap/docs/java/request-contexts#global-providers

## Handling Errors
- https://cap.cloud.sap/docs/java/indicating-errors
- Go thorugh it for more details

## Configuring endpoints for different Protocol
- https://cap.cloud.sap/docs/java/application-services#configure-endpoints

## Application Services
- Link : https://cap.cloud.sap/docs/java/application-services#configure-endpoints

  
Application Services define the APIs that a CAP application exposes to its clients, for example through OData. This section describes how to add business logic to these services, by extending CRUD events and implementing actions and functions.
- Handling CRUD Events
- OData Requests
- Deeply Structured Documents
- Result Handling
  - READ Result
  - UPDATE and DELETE Results
  - INSERT and UPSERT Results
    - Result Builder
  - Actions and Functions


### Serve Configuration
Configure how application services are served. You can define per service which ones are served by which protocol adapters. In addition, you configure on which path they are available. Finally, the combined path an application service is served on, is composed of the base path of a protocol adapter and the relative path of the application service.

### Configure Base Path
By default, the CAP Java SDK provides protocol adapters for OData V4 and V2 and the base paths of both can be configured with CDS Properties in the application.yaml:
````
cds:
  odataV4.endpoint.path: '/api'
  odataV2.endpoint.path: '/api-v2'
````

### Configure Path and Protocol  
With the annotations `@endpoints.path` and `@endpoints.protocol`, you can provide more complex service endpoint configurations. Use them to serve an application service on different paths for different protocols. The value of @endpoints.path is appended to the protocol adapter's base path.

In the following example, the service CatalogService is available on different paths for the different OData protocols:

````
@endpoints: [
  {path : 'browse', protocol: 'odata-v4'},
  {path : 'list', protocol: 'odata-v2'}
]
service CatalogService {
    ...
}
````

The CatalogService is accessible on the combined path /odata/v4/browse with the OData V4 protocol and on /odata/v2/list with the OData V2 protocol.

The same can also be configured in the application.yaml.

## Persistence Service 
- https://cap.cloud.sap/docs/java/persistence-services
- The CAP Java SDK has built-in support for various databases. This section describes the different databases and any differences between them with respect to CAP features. There's out of the box support for SAP HANA with CAP currently as well as H2 and SQLite. However, it's important to note that H2 and SQLite aren't an enterprise grade database and are recommended for nonproductive use like local development or CI tests only. PostgreSQL is supported in addition, but has various limitations in comparison to SAP HANA, most notably in the area of schema evolution.
- SAP HANA (Cloud)
- PostgreSQL
- H2 Database
- SQLite
### Datasources

Java Applications usually connect to SQL databases through datasources (`java.sql.DataSource`). The CAP Java SDK can auto-configure datasources from service bindings and pick up datasources configured by Spring Boot. These datasources are used to create Persistence Services, which are CQN-based database clients.
- Data Source configs . Here you can share Hana DB specific configs. 
````
cds:
  dataSource:
    my-service-instance:
      hikari:
        data-source-properties:
          packetSize: 300000
````   
Look at this link for Datasource Configuration from here : https://cap.cloud.sap/docs/java/persistence-services#sap-hana


## Working with CDS models (introspection APIs)
- https://cap.cloud.sap/docs/java/reflection-api
### Feature toggle 
- https://cap.cloud.sap/docs/java/reflection-api#feature-toggles
- 
## CQL
### Buinding CQL statements 
- https://cap.cloud.sap/docs/java/query-api
### Executing CQL statements 
- https://cap.cloud.sap/docs/java/query-execution
### Introspecting CQL statements 
- https://cap.cloud.sap/docs/java/query-introspection
- `CqnAnalyzer` , `CqnVisitor`
- 
## Working with Data 
- https://cap.cloud.sap/docs/java/data
### Types and Mapping 
https://cap.cloud.sap/docs/java/data#predefined-types

### Navigation and association 
- https://cap.cloud.sap/docs/java/data#relationships-to-other-entities
- Explains how to use queries using teh Data Object
### CDS Data Object 
- [CDS Data](https://cap.cloud.sap/docs/java/data#cds-data)
- Serialization + Deserialization
- [How to use Data in CQL ](https://cap.cloud.sap/docs/java/data#data-in-cds-query-language-cql)

## Event Handler 
- https://cap.cloud.sap/docs/java/provisioning-api
- Standard handlers
- Special Handler
  - `HandlerOrder.LATE`
 
## All about Servic  Comsumption
- https://cap.cloud.sap/docs/java/consumption-api


## Authorization 
- https://cap.cloud.sap/docs/guides/authorization#prerequisite-authentication
