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


  



  
