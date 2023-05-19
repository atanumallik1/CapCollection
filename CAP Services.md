# LEarn about CAP Services in JAVA
## Emit() sv Run()

Services dispatch events to `Event Handlers`, which implement the behaviour of the service. A service can process synchronous as well as asynchronous events and offers a user-friendly API layer around these events.

Every service implements the `Service` interface, which offers generic event processing capabilities through its `emit(EventContext)` method. The Event Context contains information about the event and its parameters. The `emit` method takes care of dispatching an Event Context to all event handlers registered on the respective event and is the central API to process asynchronous and synchronous events.

Usually service implementations extend the Service interface to provide a custom, u**ser-friendly API layer on top of the emit() method**. Examples are the `Application Service`, `Persistence Service`, and `Remote Service`, which offer a common CQN query execution API for their CRUD events. They Offer [`run`](https://www.javadoc.io/doc/com.sap.cds/cds-services-api/latest/com/sap/cds/services/cds/CqnService.html#run-com.sap.cds.ql.cqn.CqnDelete-java.lang.Iterable-) method.

However, also technical components are implemented as services, for example the AuthorizationService or the MessagingService.


## Using Services : ServiceCatalog
Often times your Java code needs to interact with other services. The `ServiceCatalog` provides programmatic access to all available services. The Service Catalog can be accessed from the `Event Context` or from the `CdsRuntime`.
````java
ServiceCatalog catalog = context.getServiceCatalog();
Stream<Service> allServices = catalog.getServices();
Stream<ApplicationService> appServices = catalog.getServices(ApplicationService.class);
````
To look up a service in the Service Catalog, you need to know its name. Application Services are created with the fully qualified name of their CDS definition by default:
````java
ApplicationService adminService = catalog.getService(ApplicationService.class, "AdminService");
````
Technical services, like the Persistence Service have a DEFAULT_NAME constant defined in their interface:
````java
PersistenceService db = catalog.getService(PersistenceService.class, PersistenceService.DEFAULT_NAME);
````

## Triggering Custom Events ( For Actions + Functions )
How to call a Function from another service 
````java
import static bookshop.Bookshop_.BOOKS;

@Component
public class CatalogServiceAPI {

    @Autowired
    @Qualifier(CatalogService_.CDS_NAME)
    CqnService catalogService; // get access to the service

    public Reviews review(String bookId, Integer stars) {
        ReviewEventContext context = ReviewEventContext.create();
        context.setCqn(Select.from(BOOKS).byId(bookId)); // set target entity
        context.setStars(stars); // set input parameters
        catalogService.emit(context); // emit the event
        return context.getResult(); // return the result
    }

}
````



## Reference 
- https://cap.cloud.sap/docs/java/consumption-api#an-event-based-api
